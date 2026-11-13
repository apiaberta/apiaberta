# Architecture

## Overview

```
  Internet
  api.apiaberta.pt
        │
        ▼
┌───────────────────────────────┐
│ nginx (reverse proxy, TLS)    │
└──────────────┬────────────────┘
               ▼
┌───────────────────────────────────────────────┐
│ Gateway (PM2)  @apiaberta/gateway             │
│                                               │
│  - Rate limiting (per IP / per API key)       │
│  - API key + JWT auth for account routes      │
│  - In-memory response cache (TTL per service) │
│  - Usage logging, webhooks, status history    │
│  - HTTP proxy → connectors (strips /v1)       │
│  - Swagger UI at /docs                        │
└───┬────────┬────────┬───────── … ────────┬────┘
    │        │        │                    │
    ▼        ▼        ▼                    ▼
 connector connector connector   …      connector
  -fuel     -ipma     -base               -sns
    │        │        │                    │
    └────────┴────────┴──── MongoDB ────────┘
             (one database per service)
```

## Components

### 1. Gateway (`@apiaberta/gateway`)

The single public entry point for all API consumers. Repository: [gateway](https://github.com/apiaberta/gateway).

- **Stack:** Node.js 22 + Fastify 5 + MongoDB (Mongoose)
- **Managed by:** PM2 (`apiaberta-gateway` process, port 4000 in production, 3000 in development)

**Responsibilities:**
- Rate limiting: 30 req/min per IP without an API key, 300 req/min with one
- Authenticates account routes (JWT or `X-API-Key`); data routes are public
- Caches successful `GET` responses in memory, per URL, with a TTL per service
- Proxies `/v1/<service>/*` to the service as `/<service>/*` (the `/v1` prefix is stripped)
- Records usage, runs webhook delivery, and keeps a 90-day uptime history for the status page
- Exposes Swagger UI at `/docs`

**Main routes:**
```
GET  /health                    → healthcheck (public)
GET  /docs                      → Swagger UI (public)
GET  /v1/status                 → live health of every service (public)
GET  /v1/status/history         → daily uptime history (public)
GET  /v1/status/incidents       → degraded/down incidents (public)
GET  /v1/stats                  → aggregated platform statistics (public)
POST /v1/auth/register|login    → accounts and API keys
GET  /v1/auth/me                → account info (authenticated)
     /v1/webhooks/*             → webhook subscriptions (authenticated)
     /v1/admin/*                → admin only
GET  /v1/<service>/*            → proxy → connector (see below)
```

### 2. Connectors

Independent services — one per data source, one repository each (`connector-<name>`). Each connector:

- Has its own port and PM2 process
- Exposes an internal REST API consumed by the Gateway
- Runs a cron job to refresh data from the upstream source
- Reads/writes its own MongoDB database
- Is stateless and restartable

**Currently deployed** (default internal ports from the gateway configuration; production values come from environment variables):

| Service | Public prefix | Default port | Source |
|---------|---------------|--------------|--------|
| connector-fuel | `/v1/fuel` | 3001 | DGEG |
| connector-ipma | `/v1/ipma` | 3002 | IPMA |
| connector-base | `/v1/base`, `/v1/contracts` | 3003 | BASE (public contracts) |
| connector-ev | `/v1/ev` | 3004 | ERSE / OMIE |
| connector-ine | `/v1/ine` | 3005 | INE / Eurostat |
| connector-anpc | `/v1/anpc` | 3006 | ANEPC / fogos.pt |
| connector-bdp | `/v1/bdp` | 3008 | Banco de Portugal |
| connector-geo | `/v1/geo` | 3009 | geoapi.pt |
| connector-nasafirms | `/v1/nasafirms` | 3012 | NASA FIRMS |
| connector-nif | `/v1/nif` | 3013 | MOD 11 / VIES |
| connector-prr | `/v1/prr`, `/v1/pt2030` | 3014 | transparencia.gov.pt |
| connector-dre | `/v1/dre` | 3015 | dre.pt |
| connector-carris | `/v1/carris` | 3017 | Carris Metropolitana |
| connector-gtfs | `/v1/gtfs` | 3018 | GTFS operators |
| connector-parlamento | `/v1/parlamento` | 3019 | Assembleia da República |
| connector-sns | `/v1/sns` | 3020 | transparencia.sns.gov.pt |

`connector-pt2030` also exists as a repository, but the gateway sends both `/v1/prr` and `/v1/pt2030` to the PRR service.

**Planned:** see [data-sources.md](data-sources.md) (for example APA air quality).

### 3. Database

- **MongoDB 7** running locally next to the services
- Each service uses its own database (not shared)
- Collections named by service and data type

| Database | Owner | Notes |
|----------|-------|-------|
| `apiaberta-gateway` | gateway | accounts, usage logs, webhooks, status history |
| one per connector | connector | e.g. `apiaberta-fuel`; each connector's `.env.example` shows its default |

### 4. Infrastructure

| Layer | Technology | Notes |
|-------|------------|-------|
| Hosting | Single server | All services |
| Process manager | PM2 | `ecosystem.config.cjs` in each repository |
| Reverse proxy | nginx | TLS termination, host routing |
| Runtime | Node.js 22 | ESM modules |
| Framework | Fastify 5 | All services |
| Database | MongoDB 7 | Local instance |
| Cache | In-memory (gateway) | Per process; no Redis |
| CI | GitHub Actions | Tests on push and pull request in the gateway, the frontends and several connectors |
| Docs | Swagger UI | `/docs` on the gateway |

Deployment is done by hand with PM2; the gateway repository no longer has an automatic deploy workflow.

## Cache

The gateway keeps an in-memory cache of successful `GET` responses:

- Key: HTTP method + public URL (aliases such as `/v1/contracts` share entries with `/v1/base`)
- TTL per service prefix, for example 20 s for `/v1/carris`, 60 s for `/v1/anpc`, 600 s for `/v1/fuel`, 3600 s for `/v1/ine`
- Response headers: `X-Cache: HIT | MISS`, `X-Cache-Remaining`, `Cache-Control`

The cache lives inside each gateway process. If the gateway runs in PM2 cluster mode every worker has its own copy. A shared cache (Redis) is not implemented.

## Principles

1. **Services communicate only via HTTP** — no direct access to other services' databases
2. **Consumer-Driven Contracts** — the Gateway defines the API format; connectors adapt
3. **Data first** — better to have few correct data points than many inconsistent ones
4. **Independent deployment** — each service can be updated and restarted independently
5. **Connectors are read-only to consumers** — only the connector itself writes to its DB

## Deployment (PM2)

```bash
# In each service repository
cp .env.example .env        # fill in the values
npm ci --omit=dev
pm2 start ecosystem.config.cjs

pm2 save
pm2 startup
```

nginx forwards `api.apiaberta.pt` to the gateway's local port.
