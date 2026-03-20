# 🇵🇹 API Aberta

**A unified, modern and open API for Portuguese public data.**

API Aberta aggregates and normalises scattered Portuguese government data into a single coherent REST interface — with clear documentation, free API keys and stable endpoints.

[![Status](https://img.shields.io/badge/status-live-brightgreen)](https://api.apiaberta.pt)
[![Docs](https://img.shields.io/badge/docs-api.apiaberta.pt-blue)](https://api.apiaberta.pt/docs)
[![License](https://img.shields.io/badge/license-MIT-blue)](LICENSE)

🌐 **Website:** [apiaberta.pt](https://apiaberta.pt)  
📖 **API Docs:** [api.apiaberta.pt/docs](https://api.apiaberta.pt/docs)  
🔑 **Dev Portal:** [app.apiaberta.pt](https://app.apiaberta.pt)

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
  IPMA, BdP)   extract)     deduplicate)            API keys)
```

- **Normalised data** in modern JSON
- **Stable, versioned endpoints** independent of the original sources
- **Low latency** — data is pre-processed and cached
- **Free API keys** with generous rate limiting
- **Interactive documentation** via Swagger

---

## Data sources

| Source | Description | Endpoints | Status |
|--------|-------------|-----------|--------|
| [DGEG](https://www.dgeg.gov.pt) | Fuel prices | `/v1/fuel/*` | ✅ Live |
| [IPMA](https://www.ipma.pt) | Weather & forecasts | `/v1/weather/*` | ✅ Live |
| [MOBI.E](https://www.mobie.pt) | EV charging stations | `/v1/ev/*` | ✅ Live |
| [INE](https://www.ine.pt) | National statistics | `/v1/ine/*` | ✅ Live |
| [ANPC](https://www.prociv.pt) | Civil protection alerts | `/v1/anpc/*` | ✅ Live |
| [BdP](https://www.bportugal.pt) | Interest rates | `/v1/bdp/*` | ✅ Live |
| [CTT](https://www.ctt.pt) | Geographic data | `/v1/geo/*` | ✅ Live |
| [Portal Base](https://www.base.gov.pt) | Public contracts | `/v1/base/*` | 🚧 Blocked (awaiting API access) |

---

## Quick start

### Without API key (30 req/min)

```bash
# Get current fuel prices
curl https://api.apiaberta.pt/v1/fuel/prices

# Get weather for Lisbon
curl https://api.apiaberta.pt/v1/weather/forecast/1110600

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

## JavaScript SDK

```bash
npm install apiaberta
```

```javascript
import { ApiAberta } from 'apiaberta'

const api = new ApiAberta({ apiKey: 'YOUR_KEY' })

// Fuel prices
const prices = await api.fuelPrices()

// Weather forecast
const weather = await api.weatherForecast('1110600')

// EV charging stations
const stations = await api.evStations({ district: 'Lisboa' })
```

📦 [NPM Package](https://www.npmjs.com/package/apiaberta) · [SDK Docs](https://apiaberta.pt/sdk)

---

## Project structure

| Repository | Description |
|------------|-------------|
| [gateway](https://github.com/apiaberta/gateway) | Main API gateway (auth, routing, rate limiting) |
| [connector-fuel](https://github.com/apiaberta/connector-fuel) | DGEG fuel prices connector |
| [connector-ipma](https://github.com/apiaberta/connector-ipma) | IPMA weather connector |
| [connector-ev](https://github.com/apiaberta/connector-ev) | MOBI.E EV charging connector |
| [connector-ine](https://github.com/apiaberta/connector-ine) | INE statistics connector |
| [connector-anpc](https://github.com/apiaberta/connector-anpc) | ANPC civil protection connector |
| [connector-bdp](https://github.com/apiaberta/connector-bdp) | Bank of Portugal rates connector |
| [connector-geo](https://github.com/apiaberta/connector-geo) | Geographic data connector |
| [apiaberta.pt](https://github.com/apiaberta/apiaberta.pt) | Landing page |
| [app.apiaberta.pt](https://github.com/apiaberta/app.apiaberta.pt) | Developer portal |
| [sdk-js](https://github.com/apiaberta/sdk-js) | JavaScript SDK |

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

See each repository's README for specific configuration.

---

## Contributing

All contributions are welcome — especially connectors for new data sources.

See [CONTRIBUTING.md](CONTRIBUTING.md) to get started.

**Ideas for new connectors:**
- DRE (Official Gazette)
- APA (Environmental data)
- IMT (Transport/vehicles)
- DGADR (Agriculture)

---

## License

MIT — free to use, modify and distribute.

---

Made with ❤️ in Portugal
