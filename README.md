# 🇵🇹 API Aberta

**A unified, modern and open API for Portuguese public data.**

API Aberta aggregates and normalises scattered Portuguese government data into a single coherent REST interface — with clear documentation, free API keys and stable endpoints.

[![Status](https://img.shields.io/badge/status-live-brightgreen)](https://status.apiaberta.pt)
[![Docs](https://img.shields.io/badge/docs-api.apiaberta.pt-blue)](https://api.apiaberta.pt/docs)
[![License](https://img.shields.io/badge/license-MIT-blue)](LICENSE)

🌐 **Website:** [apiaberta.pt](https://apiaberta.pt)  
📖 **API Docs:** [api.apiaberta.pt/docs](https://api.apiaberta.pt/docs)  
🔑 **Dev Portal:** [app.apiaberta.pt](https://app.apiaberta.pt)  
📡 **Status:** [status.apiaberta.pt](https://status.apiaberta.pt)

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
Gov Source → Connector → Database → Gateway → Public API
 (DGEG, INE,  (normalise,  (MongoDB,   (API keys,  (REST/JSON,
  IPMA, BdP)   cron sync)   one per     rate limit,  /v1/...)
                            service)    cache)
```

- **Normalised data** in modern JSON
- **Stable, versioned endpoints** independent of the original sources
- **Low latency** — data is pre-processed and cached
- **Free API keys** with generous rate limiting
- **Interactive documentation** via Swagger UI

---

## Data sources

| Source | Description | Endpoints | Status |
|--------|-------------|-----------|--------|
| [DGEG](https://www.dgeg.gov.pt) | Fuel prices | `/v1/fuel/*` | ✅ Live |
| [IPMA](https://www.ipma.pt) | Weather forecasts, warnings, seismic events | `/v1/ipma/*` | ✅ Live |
| [BASE](https://www.base.gov.pt) | Public contracts (`/v1/contracts/*` is an alias) | `/v1/base/*` | ✅ Live |
| [ERSE](https://www.erse.pt) / [OMIE](https://www.omie.es) | EV charging tariffs (CEME) and spot electricity prices | `/v1/ev/*` | ✅ Live |
| [INE](https://www.ine.pt) / Eurostat | National statistics | `/v1/ine/*` | ✅ Live |
| [ANEPC](https://www.prociv.pt) / [fogos.pt](https://fogos.pt) | Civil protection incidents, fire risk, warnings | `/v1/anpc/*` | ✅ Live |
| [BdP](https://www.bportugal.pt) | Interest rates | `/v1/bdp/*` | ✅ Live |
| [geoapi.pt](https://geoapi.pt) | Geographic data (districts, municipalities, parishes, postal codes) | `/v1/geo/*` | ✅ Live |
| MOD 11 / VIES | NIF validation and company lookup | `/v1/nif/*` | ✅ Live |
| [transparencia.gov.pt](https://transparencia.gov.pt) | PRR / Portugal 2030 European funds | `/v1/prr/*`, `/v1/pt2030/*` | ✅ Live |
| [DRE](https://dre.pt) | Republic legislation | `/v1/dre/*` | ✅ Live |
| [NASA FIRMS](https://firms.modaps.eosdis.nasa.gov) | Satellite fire detection | `/v1/nasafirms/*` | ✅ Live |
| [Carris Metropolitana](https://www.carrismetropolitana.pt) | Real-time vehicles, stops, lines, alerts | `/v1/carris/*` | ✅ Live |
| GTFS operators | Unified transport schedules | `/v1/gtfs/*` | ✅ Live |
| [Assembleia da República](https://www.parlamento.pt) | Initiatives, votes, MPs, petitions | `/v1/parlamento/*` | ✅ Live |
| [SNS Transparência](https://transparencia.sns.gov.pt) | Emergency, surgery, consultations, staff | `/v1/sns/*` | ✅ Live |

Platform endpoints: `GET /v1/status` (health of every service) and `GET /v1/stats`.

More sources are proposed in [docs/data-sources.md](docs/data-sources.md). The full endpoint catalogue is at [apiaberta.pt/docs/endpoints](https://apiaberta.pt/docs/endpoints).

---

## Quick start

### Without API key (30 req/min per IP)

```bash
# Get current fuel prices
curl https://api.apiaberta.pt/v1/fuel/prices

# Get weather for Lisbon
curl https://api.apiaberta.pt/v1/ipma/forecasts/1110600

# Get all districts
curl https://api.apiaberta.pt/v1/geo/districts
```

### With API key (300 req/min)

1. Register at [app.apiaberta.pt](https://app.apiaberta.pt)
2. Get your free API key
3. Include in requests:

```bash
curl -H "X-API-Key: YOUR_KEY" https://api.apiaberta.pt/v1/fuel/prices
```

---

## SDKs and tools

| Repository | What it is |
|------------|------------|
| [apiaberta-js](https://github.com/apiaberta/apiaberta-js) | JavaScript / TypeScript SDK |
| [apiaberta-py](https://github.com/apiaberta/apiaberta-py) | Python SDK |
| [apiaberta-cli](https://github.com/apiaberta/apiaberta-cli) | Command-line client |
| [MCP server](https://github.com/apiaberta/apiaberta.pt/tree/main/mcp) | Model Context Protocol server for Claude and Cursor (lives in the website repo) |

> **Not published yet.** As of November 2026 the SDKs and the CLI are not on npm or PyPI, so `npm install apiaberta` and `pip install apiaberta` do not work. Install from the GitHub repositories and see each README. The SDKs cover a subset of services (fuel, weather, civil protection, EV, statistics, contracts, BdP, geo); the REST API works without any SDK.

```javascript
import ApiAberta from 'apiaberta-js'

const api = new ApiAberta({ apiKey: 'YOUR_KEY' })

const prices = await api.fuel({ district: 'Lisboa' })
const weather = await api.weather({ days: 3 })
const incidents = await api.activeIncidents()
```

For LLMs and agents: [llms.txt](https://apiaberta.pt/llms.txt) and [llms-full.txt](https://apiaberta.pt/llms-full.txt).

---

## Project structure

This repository holds the organisation-level documentation, issue templates and contribution guides. The code lives in separate repositories:

| Repository | Description |
|------------|-------------|
| [gateway](https://github.com/apiaberta/gateway) | Public API gateway (auth, rate limiting, cache, routing, webhooks, status) |
| [service-template](https://github.com/apiaberta/service-template) | Starting point for a new connector |
| [connector-fuel](https://github.com/apiaberta/connector-fuel) | DGEG fuel prices |
| [connector-ipma](https://github.com/apiaberta/connector-ipma) | IPMA weather and seismic data |
| [connector-base](https://github.com/apiaberta/connector-base) | BASE public contracts |
| [connector-ev](https://github.com/apiaberta/connector-ev) | EV charging tariffs and OMIE prices |
| [connector-ine](https://github.com/apiaberta/connector-ine) | INE / Eurostat statistics |
| [connector-anpc](https://github.com/apiaberta/connector-anpc) | Civil protection incidents |
| [connector-bdp](https://github.com/apiaberta/connector-bdp) | Bank of Portugal rates |
| [connector-geo](https://github.com/apiaberta/connector-geo) | Geographic data |
| [connector-nif](https://github.com/apiaberta/connector-nif) | NIF validator |
| [connector-prr](https://github.com/apiaberta/connector-prr) | PRR / Portugal 2030 funds |
| [connector-pt2030](https://github.com/apiaberta/connector-pt2030) | PT2030 variant of the PRR connector |
| [connector-dre](https://github.com/apiaberta/connector-dre) | DRE legislation |
| [connector-nasafirms](https://github.com/apiaberta/connector-nasafirms) | NASA FIRMS fire hotspots |
| [connector-carris](https://github.com/apiaberta/connector-carris) | Carris Metropolitana real-time |
| [connector-gtfs](https://github.com/apiaberta/connector-gtfs) | GTFS schedules |
| [connector-parlamento](https://github.com/apiaberta/connector-parlamento) | Parliament data |
| [connector-sns](https://github.com/apiaberta/connector-sns) | SNS transparency data |
| [apiaberta.pt](https://github.com/apiaberta/apiaberta.pt) | Landing page, docs and MCP server |
| [app.apiaberta.pt](https://github.com/apiaberta/app.apiaberta.pt) | Developer portal |
| [status.apiaberta.pt](https://github.com/apiaberta/status.apiaberta.pt) | Public status page |
| [apiaberta-tests](https://github.com/apiaberta/apiaberta-tests) | Playwright tests |
| [apiaberta-js](https://github.com/apiaberta/apiaberta-js) · [apiaberta-py](https://github.com/apiaberta/apiaberta-py) · [apiaberta-cli](https://github.com/apiaberta/apiaberta-cli) | SDKs and CLI |

See [docs/architecture.md](docs/architecture.md) for how the pieces fit together.

---

## Self-hosting

Each connector runs independently. Example:

```bash
git clone https://github.com/apiaberta/connector-fuel
cd connector-fuel
cp .env.example .env
npm install
npm start
```

Requires Node.js 22+ and MongoDB 7+. See each repository's README for specific configuration.

---

## Contributing

All contributions are welcome — especially connectors for new data sources and documentation fixes.

See [CONTRIBUTING.md](CONTRIBUTING.md) to get started.

**Ideas for new connectors** (see [docs/data-sources.md](docs/data-sources.md)):
- APA (air quality)
- ERSE (electricity and gas tariffs)
- IMT (transport/vehicles)
- DGADR (agriculture)

---

## License

MIT — free to use, modify and distribute.

---

Made with ❤️ in Portugal
