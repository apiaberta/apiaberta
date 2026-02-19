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

Every service MUST implement these two endpoints:

### `GET /health`
Returns the health status of the service. Used by the gateway and monitoring.

```json
{
  "status": "ok",
  "service": "fuel",
  "timestamp": "2026-03-01T12:00:00Z"
}
```

### `GET /meta`
Returns metadata about the data this service provides.

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

- All data routes MUST be under `/v1/`
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
- In production, bind to internal network only (`host: '0.0.0.0'`, firewall rules applied by infra)
- Strip any headers forwarded from clients before using them as data
- The gateway injects `x-developer-tier` - you may use this for tier-specific responses

---

## 8. Pagination

Support these query parameters on list endpoints:

| Parameter | Type | Default | Max | Description |
|-----------|------|---------|-----|-------------|
| `page` | integer | 1 | - | Page number |
| `limit` | integer | 20 | 100 | Results per page |
| `updated_since` | ISO date | - | - | Filter by update time |

---

## 9. Dockerfile

Every service MUST include a `Dockerfile`:

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
- A sample response from `/meta`

The gateway team will add a proxy route and your service goes live.

---

## Checklist before submitting

- [ ] `GET /health` returns `{ status: "ok" }`
- [ ] `GET /meta` returns correct metadata
- [ ] All responses use the standard envelope `{ meta, data }`
- [ ] Field names are English, snake_case
- [ ] Dates are ISO 8601
- [ ] Cron job fetches data on a schedule
- [ ] Stale data is served if source is down
- [ ] `Dockerfile` included
- [ ] README explains the data source and endpoints
- [ ] No sensitive data or source credentials in the repo
