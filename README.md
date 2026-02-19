# 🇵🇹 API Aberta

**A unified, modern and open API for Portuguese public data.**

API Aberta aggregates and normalises scattered Portuguese government data into a single coherent REST interface — with clear documentation, free API keys and stable endpoints.

[![Status](https://img.shields.io/badge/status-under%20construction-yellow)](https://github.com/apiaberta)
[![Stack](https://img.shields.io/badge/stack-Node%20%7C%20Fastify%20%7C%20MongoDB-green)](https://github.com/apiaberta/api)
[![License](https://img.shields.io/badge/license-MIT-blue)](LICENSE)

---

## The problem

Portuguese government APIs are:
- **Fragmented** — every entity has its own API (or none at all)
- **Inconsistent** — different formats, different authentication, missing documentation
- **Unstable** — SOAP, XML, WSDL from 2003, frequent downtimes
- **Slow** — 2–5 second latency is common

## The solution

API Aberta works as an abstraction layer:

```
Gov Source → Connector → Ingest → Database → Public API
 (DGEG, INE,  (normalise,  (validate,  (MongoDB)  (REST/JSON,
  DRE, Base)   extract)     deduplicate)            API keys)
```

- **Normalised data** in modern JSON
- **Stable, versioned endpoints** independent of the original sources
- **Low latency** — data is pre-processed and stored
- **Free API keys** with generous rate limiting
- **Interactive documentation** via Swagger

---

## Data sources

| Source | Description | Status |
|--------|-------------|--------|
| [DGEG](https://www.dgeg.gov.pt) | Fuel prices | 🔄 In development |
| [Portal Base](https://www.base.gov.pt) | Public contracts | 📋 Planned |
| [INE](https://www.ine.pt) | National statistics | 📋 Planned |
| [DRE](https://dre.pt) | Official gazette | 📋 Planned |
| [IPMA](https://www.ipma.pt) | Weather | 📋 Planned |

---

## Project structure

```
apiaberta/
├── api/          → Fastify API (public endpoints, auth, rate limiting)
├── connectors/   → Data connectors per source
├── ingest/       → Validation and normalisation service
└── docs/         → Technical documentation
```

Each component is an independent service with its own repository.

---

## Roadmap

See [docs/roadmap.md](docs/roadmap.md) for the detailed plan.

**Phase 1 — MVP (March 2026)**
- [ ] DGEG connector (fuel prices)
- [ ] Fastify API with `/v1/fuel/prices` endpoint
- [ ] API key system
- [ ] Swagger documentation

**Phase 2 — Growth (April–June 2026)**
- [ ] Developer registration portal
- [ ] 3+ additional connectors
- [ ] Tiered rate limiting
- [ ] Usage dashboard

---

## Contributing

All contributions are welcome — especially connectors for new data sources.

See [CONTRIBUTING.md](CONTRIBUTING.md) to get started.

---

## License

MIT — free to use, modify and distribute.
