# Roadmap

## Phase 1 — MVP (March 2026)

Goal: have a working public endpoint with real data.

### Milestone 1.1 — Base structure
- [ ] Repositories created (`api`, `connectors`, `ingest`)
- [ ] Node + Fastify + MongoDB setup
- [ ] Healthcheck endpoint (`GET /health`)
- [ ] Basic deployment on Railway

### Milestone 1.2 — First connector
- [ ] DGEG connector — fuel prices
- [ ] Daily cron job
- [ ] Data flowing into MongoDB

### Milestone 1.3 — Public API v1
- [ ] `GET /v1/fuel/prices` — national average prices
- [ ] `GET /v1/fuel/stations` — stations by location
- [ ] Swagger documentation
- [ ] API keys (simple email registration)

---

## Phase 2 — Growth (April–June 2026)

### Milestone 2.1 — Developer portal
- [ ] API key registration page
- [ ] Usage dashboard
- [ ] Tiered rate limiting (Free / Pro)

### Milestone 2.2 — More connectors
- [ ] Portal Base — public contracts
- [ ] INE — economic indicators
- [ ] DRE — recent legislation

### Milestone 2.3 — Quality
- [ ] Automated tests (CI/CD)
- [ ] Connector failure alerts
- [ ] Public SLA (uptime page)

---

## Phase 3 — Scale (H2 2026)

- [ ] Open external contributions for new connectors
- [ ] Programmatic API (Chave Móvel Digital)
- [ ] SDKs (JavaScript, Python)
- [ ] Paid tier with guaranteed SLA

---

## Out of scope (for now)

- Sensitive data (health records, personal finances)
- Citizen authentication
- Integration with internal government systems
