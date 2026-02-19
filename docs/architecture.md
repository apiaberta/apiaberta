# Architecture

## Overview

```
                         ┌─────────────────────────────────────┐
                         │           VPS (167.99.216.205)       │
                         │                                      │
  Internet               │  ┌────────────────────────────────┐  │
  ──────────────────────▶│  │   nginx (reverse proxy)        │  │
  api.apiaberta.pt        │  │   :80 / :443 → :4000           │  │
                         │  └────────────┬───────────────────┘  │
                         │               │                      │
                         │  ┌────────────▼───────────────────┐  │
                         │  │   Gateway (PM2)                 │  │
                         │  │   @apiaberta/gateway :4000      │  │
                         │  │                                 │  │
                         │  │  - X-API-Key auth               │  │
                         │  │  - Rate limiting (per tier)     │  │
                         │  │  - Usage logging → MongoDB      │  │
                         │  │  - HTTP proxy → connectors      │  │
                         │  └──┬────────────────────────────┬┘  │
                         │     │ /v1/fuel/*                  │   │
                         │     │                             │   │
                         │  ┌──▼─────────────────┐          │   │
                         │  │ connector-fuel (PM2)│          │   │
                         │  │ :3001               │          │   │
                         │  │                     │          │   │
                         │  │  - Fastify REST API  │          │   │
                         │  │  - Cron 07:30 Lisbon │          │   │
                         │  │  - DGEG scraper      │          │   │
                         │  └──────────┬──────────┘          │   │
                         │             │                      │   │
                         │  ┌──────────▼──────────┐          │   │
                         │  │  MongoDB :27017       │          │   │
                         │  │  apiaberta-gateway    │          │   │
                         │  │  apiaberta-fuel       │          │   │
                         │  └──────────────────────┘          │   │
                         └─────────────────────────────────────┘
```

## Components

### 1. Gateway (`@apiaberta/gateway`)

The single public entry point for all API consumers.

- **Port (production):** 4000 (behind nginx on :80/:443)
- **Port (dev):** 3000
- **Managed by:** PM2 (`apiaberta-gateway` process)
- **Stack:** Node.js 22 + Fastify 5 + MongoDB (Mongoose)

**Responsibilities:**
- Validates `X-API-Key` header on all authenticated routes
- Enforces rate limits per tier (Free / Pro / Admin)
- Logs every request to MongoDB for usage tracking
- Proxies requests to internal connector services via `@fastify/http-proxy`
- Exposes Swagger docs at `/docs`

**Routes registered:**
```
GET  /health                        → healthcheck (public)
GET  /docs                          → Swagger UI (public)
POST /v1/auth/register              → get an API key (public)
GET  /v1/auth/me                    → account info (authenticated)
GET  /v1/fuel/*                     → proxy → connector-fuel :3001
GET  /v1/admin/*                    → admin only
```

**Planned (not yet active):**
```
GET  /v1/contracts/*                → proxy → connector-base :3002
GET  /v1/statistics/*               → proxy → connector-ine :3003
GET  /v1/legislation/*              → proxy → connector-dre :3004
```

### 2. Connectors

Independent services — one per data source. Each connector:

- Has its own port and PM2 process
- Exposes an internal REST API consumed by the Gateway
- Runs a cron job to refresh data from the upstream source
- Reads/writes its own MongoDB database
- Is stateless and restartable

**Currently deployed:**

| Connector | Port | Source | Status |
|-----------|------|--------|--------|
| connector-fuel | :3001 | DGEG precoscombustiveis.dgeg.gov.pt | ✅ Live |

**Planned:**

| Connector | Port | Source | Status |
|-----------|------|--------|--------|
| connector-base | :3002 | base.gov.pt (contratos públicos) | 🔲 Planned |
| connector-ipma | :3003 | api.ipma.pt (meteorologia) | 🔲 Planned |
| connector-ine | :3004 | ine.pt (estatísticas) | 🔲 Planned |
| connector-apa | :3005 | qualar.apambiente.pt (qualidade do ar) | 🔲 Planned |

### 3. Database

- **MongoDB 7** running locally on the VPS
- Each service uses its own database (not shared)
- Collections named by service and data type

| Database | Owner | Collections |
|----------|-------|-------------|
| `apiaberta-gateway` | gateway | `developers`, `usagelogs` |
| `apiaberta-fuel` | connector-fuel | `fuelstations`, `fuelsummaries` |

### 4. Infrastructure

| Layer | Technology | Notes |
|-------|------------|-------|
| VPS | Ubuntu (167.99.216.205) | Single server — all services |
| Process manager | PM2 | Auto-restart, log rotation |
| Reverse proxy | nginx | TLS termination, host routing |
| Runtime | Node.js 22 | ESM modules |
| Framework | Fastify 5 | All services |
| Database | MongoDB 7 | Local instance |
| Cache | — | Redis **planned**, not yet implemented |
| CI/CD | GitHub Actions | Auto-deploy on push to `main` |
| Docs | Swagger / Scalar | `/docs` on gateway |

## Cache (Planned)

Redis will be added as a cache layer between the Gateway and MongoDB/connectors:

- Cache `GET /v1/fuel/prices` for 1 hour (data updates once daily)
- Cache `GET /v1/fuel/stations` with query-key hashing for 15 min
- Cache `GET /v1/fuel/cheapest` for 15 min
- Invalidate on each successful connector sync

This is **not yet implemented**. All requests currently hit MongoDB directly.

## Principles

1. **Services communicate only via HTTP** — no direct access to other services' databases
2. **Consumer-Driven Contracts** — the Gateway defines the API format; connectors adapt
3. **Data first** — better to have few correct data points than many inconsistent ones
4. **Independent deployment** — each service can be updated and restarted independently
5. **Connectors are read-only to consumers** — only the connector itself writes to its DB

## Deployment (PM2)

```bash
# On the VPS
cd /root/.openclaw/workspace/gateway
pm2 start ecosystem.config.cjs

cd /root/.openclaw/workspace/connector-fuel
pm2 start ecosystem.config.cjs   # or: pm2 start src/index.js --name connector-fuel

pm2 save
pm2 startup
```

nginx config: `api.apiaberta.pt` → `proxy_pass http://127.0.0.1:4000`
