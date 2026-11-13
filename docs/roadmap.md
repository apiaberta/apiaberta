# API Aberta — Roadmap

Status below reflects the public repositories and the live API as of November 2026. Dates in the phase titles are the original plan.

## ✅ Phase 1 — MVP (March 2026) — COMPLETE

- [x] DGEG connector (fuel prices)
- [x] IPMA connector (weather & forecasts)
- [x] EV connector (CEME tariffs and OMIE spot prices)
- [x] INE connector (statistics)
- [x] ANPC connector (civil protection)
- [x] BdP connector (interest rates)
- [x] Geographic connector (districts, municipalities, parishes, postal codes — via geoapi.pt)
- [x] Fastify API gateway with versioned endpoints
- [x] API key system with rate limiting
- [x] Swagger documentation
- [x] Developer portal (app.apiaberta.pt)
- [x] Landing page (apiaberta.pt)
- [x] JavaScript SDK (repository: apiaberta-js — not yet published to npm)

## 🚧 Phase 2 — Growth (planned April–June 2026) — PARTLY DONE

- [x] Portal Base connector (public contracts) — live at `/v1/base`
- [x] DRE connector (Official Gazette) — live at `/v1/dre`
- [x] Webhooks (`ipma.warning.new`, `fuel.prices.updated`, `anpc.incident.new`, `base.contract.new`, `ev.prices.updated`)
- [x] Usage analytics dashboard — per-developer usage in the portal dashboard (`/v1/auth/usage`) and an admin usage page (`/v1/admin/usage`)
- [ ] Tiered rate limiting (free/pro) — today the limit depends only on whether an API key is sent (30 vs 300 req/min)
- [ ] Email notifications for API changes

### Also shipped (not in the original plan)

- [x] NIF validator, PRR / Portugal 2030, NASA FIRMS, Carris Metropolitana, GTFS, Parlamento and SNS connectors
- [x] Public status page (status.apiaberta.pt) with 90-day uptime history
- [x] In-memory response cache in the gateway
- [x] Python SDK and command-line client (repositories: apiaberta-py, apiaberta-cli — not yet published)
- [x] MCP server and `llms.txt` / `llms-full.txt` for agents

### Next up

- [ ] Publish the SDKs and the CLI to npm and PyPI
- [ ] Extend the SDKs and CLI to the newer services (NIF, PRR, DRE, FIRMS, Carris, GTFS, Parlamento, SNS)

## 📋 Phase 3 — Expansion (planned Q3 2026) — NOT STARTED

No dedicated code was found in the public repositories for these items.

- [ ] Additional connectors (APA, IMT, DGADR)
- [ ] GraphQL endpoint
- [ ] Historical data access
- [ ] Data export (CSV, Excel)
- [ ] Mobile SDK (React Native)

## 💡 Ideas / Backlog

- Real-time fuel price alerts
- Integration with Google Sheets
- Zapier/Make connectors
- Community-contributed connectors
- Premium tier with SLA

---

Last updated: November 2026
