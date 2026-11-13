# Data Sources

Inventory of Portuguese public data sources for API Aberta connectors.

Status legend: **live** = available in the API | **planned** = on the roadmap | **investigate** = being evaluated | **blocked** = restricted access

---

## ⛽ Energy

| Source | URL | Data | Status | Priority |
|--------|-----|------|--------|----------|
| DGEG — Fuel prices | https://precoscombustiveis.dgeg.gov.pt/api/ | Prices by station, type, district, brand (14 types) | **live** (`/v1/fuel`) | — |
| ERSE / OMIE — EV charging | https://www.erse.pt | CEME charging tariffs and OMIE spot electricity prices | **live** (`/v1/ev`) | — |
| DGEG — ArcGIS / SIG | https://sig.dgeg.gov.pt/ | Energy infrastructure maps | investigate | low |
| ERSE — Energy regulation | https://www.erse.pt | Electricity and gas tariffs | investigate | medium |

---

## 🌤️ Weather

| Source | URL | Data | Status | Priority |
|--------|-----|------|--------|----------|
| IPMA — Forecast | https://api.ipma.pt/ | Forecasts by location, warnings, seismic events | **live** (`/v1/ipma`) | — |
| IPMA — Observations | https://api.ipma.pt/ | Weather station data (temperature, wind, humidity) | **planned** | high |
| IPMA — UV / Air quality | https://api.ipma.pt/ | UV index, particulates, ozone | investigate | medium |

---

## 📊 National statistics

| Source | URL | Data | Status | Priority |
|--------|-----|------|--------|----------|
| INE — Economic indicators | https://www.ine.pt/ine/json_api/ | GDP, inflation, unemployment, population (free registration) | **live** (`/v1/ine`) | — |
| Banco de Portugal — Interest rates | https://bpstat.bportugal.pt/api/ | ECB key rates, €STR, lending and deposit rates | **live** (`/v1/bdp`) | — |
| INE — Census | https://www.ine.pt | 2021 census data, population distribution | investigate | medium |
| Banco de Portugal — Other series | https://bpstat.bportugal.pt/api/ | Exchange rates, credit, balance of payments | investigate | medium |
| PORDATA | https://www.pordata.pt | Historical series of social and economic indicators (no formal public API) | investigate | low |

---

## 📜 Legislation, contracts and funds

| Source | URL | Data | Status | Priority |
|--------|-----|------|--------|----------|
| Portal BASE — Contracts | https://www.base.gov.pt/Base4/pt/docs/ | Public contracts, awards, entities | **live** (`/v1/base`, alias `/v1/contracts`) | — |
| DRE — Diário da República | https://dre.pt/api/v2/ | Legislation, ordinances, decrees (JSON/REST) | **live** (`/v1/dre`) | — |
| transparencia.gov.pt | https://transparencia.gov.pt | PRR and Portugal 2030 European funds | **live** (`/v1/prr`, `/v1/pt2030`) | — |
| Assembleia da República | https://www.parlamento.pt | Initiatives, votes, MPs, parliamentary groups, petitions | **live** (`/v1/parlamento`) | — |
| dados.gov.pt | https://dados.gov.pt/api/1/ | Official open data portal (CKAN API, many datasets) | investigate | medium |

---

## 🚒 Civil protection and emergencies

| Source | URL | Data | Status | Priority |
|--------|-----|------|--------|----------|
| ANEPC — Alerts and incidents | https://www.bombeiros.pt/info-estatistica | Fires, floods, real-time alerts (via fogos.pt) | **live** (`/v1/anpc`) | — |
| ANEPC — Fire risk | https://www.anepc.pt | Risk index by district/municipality | **live** (`/v1/anpc/risk`) | — |
| NASA FIRMS | https://firms.modaps.eosdis.nasa.gov | VIIRS/MODIS satellite fire hotspots | **live** (`/v1/nasafirms`) | — |
| INEM — Statistics | https://www.inem.pt | CODU calls, response times (no known public API) | investigate | medium |

---

## 🏫 Education

| Source | URL | Data | Status | Priority |
|--------|-----|------|--------|----------|
| DGEEC — Education statistics | https://www.dgeec.mec.pt | Students per school, enrolment rates, results (no formal API) | investigate | medium |
| DGES — Higher education | https://www.dges.gov.pt | Applications, places, entry grades | investigate | medium |
| ME — School map | https://dados.gov.pt | School map, types, location | investigate | low |

---

## 🏥 Health

| Source | URL | Data | Status | Priority |
|--------|-----|------|--------|----------|
| SNS — Transparency portal | https://transparencia.sns.gov.pt | Emergency, surgery, consultations and staff (monthly) | **live** (`/v1/sns`) | — |
| DGS — Epidemiological surveillance | https://www.dgs.pt | Notifiable diseases, flu, COVID (history) | investigate | medium |
| INFARMED — Medicines | https://www.infarmed.pt | Medicine prices and availability | investigate | low |

---

## 🌿 Environment

| Source | URL | Data | Status | Priority |
|--------|-----|------|--------|----------|
| APA — Air quality | https://qualar.apambiente.pt/api | Air quality index by station (JSON) | **planned** | high |
| APA — Water resources | https://snirh.apambiente.pt | Reservoir levels, flows, water quality | investigate | medium |
| ICNF — Biodiversity | https://www.icnf.pt | Protected areas, species, habitats | investigate | low |

---

## 🚌 Transport

| Source | URL | Data | Status | Priority |
|--------|-----|------|--------|----------|
| Carris Metropolitana | https://www.carrismetropolitana.pt | Real-time vehicles, stops, lines, alerts | **live** (`/v1/carris`) | — |
| GTFS operators | — | Unified schedules from Portuguese operators | **live** (`/v1/gtfs`) | — |
| IMT — Vehicles and drivers | https://www.imt-ip.pt | Registrations, driving licences, inspections (no public API) | investigate | low |
| ANAC — Civil aviation | https://www.anac.pt | Flight operations, aerodromes, certifications | investigate | medium |
| ANA Aeroportos | https://www.ana.pt | Passenger traffic, real-time flights | investigate | medium |
| CP — Comboios de Portugal | https://www.cp.pt | Timetables, fares (no formal public API) | investigate | medium |
| Metro Lisboa | — | GTFS realtime (under evaluation) | investigate | low |

---

## 🗺️ Geography

| Source | URL | Data | Status | Priority |
|--------|-----|------|--------|----------|
| geoapi.pt | https://geoapi.pt | Districts, municipalities, parishes, postal codes | **live** (`/v1/geo`) | — |

---

## 🏢 Company and judicial registers

| Source | URL | Data | Status | Priority |
|--------|-----|------|--------|----------|
| NIF (MOD 11 / VIES) | — | NIF validation and company name/address lookup | **live** (`/v1/nif`) | — |
| IRN / RNPC — National Register of Legal Entities | https://www.irn.mj.pt | NIPC, name, registered office, corporate purpose (no direct public API) | investigate | high |
| Insolv — Insolvencies | https://www.citius.mj.pt | Published insolvency proceedings | investigate | low |
| ASAE — Inspection | https://www.asae.gov.pt | Licensed establishments, inspections | investigate | low |

---

## 🗳️ Elections

| Source | URL | Data | Status | Priority |
|--------|-----|------|--------|----------|
| SGMAI — Election results | https://www.eleicoes.mai.gov.pt | Historical results of all elections by assembly/parish | investigate | medium |
| CNE | https://www.cne.pt | Electoral register, official information | investigate | low |

---

## How to propose a new source

Open an issue with the `new-source` label (use the "New Data Source" template) and include:
1. Name of the entity
2. URL of the API or data page
3. Available format (JSON, XML, CSV, SOAP…)
4. Update frequency
5. Use case (what is it for?)

---

*Last updated: November 2026*
