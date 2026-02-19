# Architecture

## Overview

```
┌─────────────┐    ┌──────────────┐    ┌──────────────┐    ┌─────────────┐
│  Gov Sources│───▶│  Connectors  │───▶│    Ingest    │───▶│  MongoDB    │
│  (DGEG,INE) │    │  (scrapers)  │    │  (validate + │    │  (source    │
│             │    │              │    │   normalise) │    │   of truth) │
└─────────────┘    └──────────────┘    └──────────────┘    └──────┬──────┘
                                                                   │
                                                           ┌───────▼──────┐
                                                           │  Public API  │
                                                           │  (Fastify)   │
                                                           │  + API Keys  │
                                                           └──────────────┘
```

## Components

### 1. Connectors (`/connectors`)

Responsible for extracting data from government sources.

- One connector per data source
- Can be scrapers, SOAP/XML clients, or REST clients
- Normalise data to the internal format
- Send data to the Ingest service via HTTP
- Run as independent cron jobs
- Stack: Node.js (preferred), any language accepted

**Rules:**
- No direct database access
- Communicate exclusively with the Ingest API
- Stateless — can be restarted at any time

### 2. Ingest (`/ingest`)

Data validation and quality control layer.

- Validates the schema of incoming data
- Deduplicates records
- Maintains version history
- Writes to MongoDB
- Stack: Node.js + Fastify

**Endpoints:**
```
POST /ingest/v1/{source}  → receives data from a connector
GET  /ingest/v1/status    → status of each data source
```

### 3. Public API (`/api`)

REST interface for external developers.

- Stable, versioned endpoints (`/v1/...`)
- API key authentication
- Rate limiting by tier
- Automatic Swagger documentation
- Stack: Node.js + Fastify + MongoDB

### 4. Database

- **MongoDB** — flexible for heterogeneous data from diverse sources
- Each source has its own collection
- Indexes optimised per use case

## Principles

1. **Services communicate only via HTTP** — no direct access to other services' databases
2. **Consumer-Driven Contracts** — the Public API defines the format it needs; connectors adapt
3. **Data first** — better to have few, correct data points than many inconsistent ones
4. **Independent deployment** — each service can be updated without affecting the others

## Stack

| Component | Technology |
|-----------|-----------|
| Runtime | Node.js 22 |
| Framework | Fastify 5 |
| Database | MongoDB 7 |
| Deployment | Railway / Render |
| CI/CD | GitHub Actions |
| Docs | Swagger / Scalar |
