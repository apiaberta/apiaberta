# Service Guidelines

Every service that joins the API Aberta ecosystem must follow these guidelines. They exist to ensure quality, consistency and reliability across the platform.

## Quick start

```bash
git clone https://github.com/apiaberta/service-template.git my-service
cd my-service
npm install
cp .env.example .env
npm run dev
```

---

## 1. Mandatory endpoints

Every service MUST implement these two endpoints.

> **Routing.** The gateway strips the leading `/v1` and keeps the service prefix: a request for `https://api.apiaberta.pt/v1/fuel/stations` reaches your service as `GET /fuel/stations`. Register your data routes and `/meta` under `/<service>/...`, and `/health` at the root. Do not register routes under `/v1` in the service itself. [service-template](https://github.com/apiaberta/service-template) is set up this way.

### `GET /health`
Returns the health status of the service. Served at the **root** (not under the service prefix). Used by the gateway (`GET /v1/status`) and monitoring.

```json
{
  "status": "ok",
  "service": "fuel",
  "timestamp": "2026-03-01T12:00:00Z"
}
```

### `GET /<service>/meta`
Returns metadata about the data this service provides. Reachable through the gateway as `/v1/<service>/meta` (for example `/v1/fuel/meta`).

```json
{
  "service": "fuel",
  "source": "https://precoscombustiveis.dgeg.gov.pt/api/",
  "description": "Fuel prices across Portugal, updated daily from DGEG",
  "last_updated": "2026-03-01T06:00:00Z",
  "record_count": 4200,
  "update_frequency": "daily"
}
```

---

## 2. Response format

All data endpoints MUST return a consistent JSON envelope:

```json
{
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 4200,
    "pages": 210
  },
  "data": [ ... ]
}
```

For single-resource endpoints:

```json
{
  "data": { ... }
}
```

Errors:

```json
{
  "error": "Not Found",
  "message": "Station with id '123' not found",
  "statusCode": 404
}
```

---

## 3. Field naming

- All field names in **English**, **snake_case**
- Dates in **ISO 8601** (`2026-03-01T06:00:00Z`)
- Prices in **EUR** (float, 3 decimal places)
- Coordinates: `{ "lat": 38.716, "lng": -9.139 }`

---

## 4. Versioning

- All data routes MUST be under `/v1/` **on the gateway** (the service itself serves them without the `/v1` prefix, see section 1)
- Breaking changes require a new version (`/v2/`)
- Old versions must remain available for at least 6 months after deprecation notice

---

## 5. Data storage

- Services store their own data in their own MongoDB instance
- Never read from another service's database directly
- Use the gateway routing for inter-service communication (rare)
- Always upsert, never blindly insert (avoid duplicates)

---

## 6. Connectors and data fetching

- Fetch data on a **cron schedule** (not in real-time per request)
- Handle source downtime gracefully - serve stale data, never fail
- Log anomalies (unexpected format changes, missing fields)
- Normalise ALL fields before storing - never expose raw source data

```js
// Good
{ "fuel_type": "gasoline_95", "price_eur": 1.729, "updated_at": "2026-03-01T06:00:00Z" }

// Bad
{ "TipoCombustivel": "Gasolina95", "Preco": "1,729", "Data": "01-03-2026" }
```

---

## 7. Security

- Services are **not publicly accessible** - they only accept traffic from the gateway
- The template listens on `host: '0.0.0.0'`, so restrict the port at network level (firewall) or set the host to `127.0.0.1` when the gateway runs on the same machine
- Strip any headers forwarded from clients before using them as data
- The gateway injects `x-developer-id` and `x-developer-tier` only when the request was authenticated. Data routes are public, so do not rely on them being present

---

## 8. Pagination

Support these query parameters on list endpoints:

| Parameter | Type | Default | Max | Description |
|-----------|------|---------|-----|-------------|
| `page` | integer | 1 | - | Page number |
| `limit` | integer | 20 | 100 | Results per page |
| `updated_since` | ISO date | - | - | Filter by update time |

New services should use `page`/`limit`. Some existing connectors (for example carris, gtfs, parlamento, sns, ipma) use `limit`/`offset` instead; keep a connector's current style until a versioned change is agreed.

---

## 9. Dockerfile

New services SHOULD include a `Dockerfile` (the template has one). Existing connectors currently run under PM2 with an `ecosystem.config.cjs` and have none:

```dockerfile
FROM node:22-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --omit=dev
COPY src/ ./src/
EXPOSE 3001
CMD ["node", "src/index.js"]
```

---

## 10. Registering your service

Once your service is live and passes the checklist below, open an issue in the [main repo](https://github.com/apiaberta/apiaberta) with the `new-connector` label and include:

- Service name
- Data source URL
- Your service's internal URL (for gateway registration)
- A sample response from `/<service>/meta`

The gateway team will add the proxy route (see [Service → Gateway registration](api-contract.md#service--gateway-registration)) and your service goes live.

---

## Checklist before submitting

- [ ] `GET /health` (root) returns `{ status: "ok" }`
- [ ] `GET /<service>/meta` returns correct metadata
- [ ] Data routes are served under `/<service>/...`, not under `/v1`
- [ ] All responses use the standard envelope `{ meta, data }`
- [ ] Field names are English, snake_case
- [ ] Dates are ISO 8601
- [ ] Cron job fetches data on a schedule
- [ ] Stale data is served if source is down
- [ ] `npm test` passes and a CI workflow runs it
- [ ] `Dockerfile` included (recommended)
- [ ] `.gitignore` excludes `node_modules/` and `.env`; `.env.example` lists every variable
- [ ] README explains the data source and endpoints
- [ ] No sensitive data or source credentials in the repo
