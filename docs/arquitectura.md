# Arquitectura

## Visão geral

```
┌─────────────┐    ┌──────────────┐    ┌──────────────┐    ┌─────────────┐
│  Fontes Gov │───▶│  Conectores  │───▶│   Ingestão   │───▶│  MongoDB    │
│  (DGEG,INE) │    │  (scrapers)  │    │  (valida +   │    │  (source    │
│             │    │              │    │   normaliza) │    │   of truth) │
└─────────────┘    └──────────────┘    └──────────────┘    └──────┬──────┘
                                                                   │
                                                           ┌───────▼──────┐
                                                           │  API Pública │
                                                           │  (Fastify)   │
                                                           │  + API Keys  │
                                                           └──────────────┘
```

## Componentes

### 1. Conectores (`/connectors`)

Responsáveis por extrair dados das fontes governamentais.

- Um conector por fonte de dados
- Podem ser scrapers, clientes SOAP/XML, ou clientes REST
- Normalizam os dados para o formato interno
- Enviam para o serviço de Ingestão via HTTP
- Correm em cron jobs independentes
- Stack: Node.js (preferencial), qualquer linguagem aceite

**Regras:**
- Não têm acesso directo à base de dados
- Comunicam exclusivamente com a API de Ingestão
- São stateless — podem ser reiniciados a qualquer momento

### 2. Ingestão (`/ingest`)

Camada de validação e controlo de qualidade dos dados.

- Valida o schema dos dados recebidos
- Remove duplicados
- Mantém histórico de versões
- Escreve no MongoDB
- Stack: Node.js + Fastify

**Endpoints:**
```
POST /ingest/v1/{source}  → recebe dados de um conector
GET  /ingest/v1/status    → estado de cada fonte
```

### 3. API Pública (`/api`)

Interface REST para developers externos.

- Endpoints estáveis e versionados (`/v1/...`)
- Autenticação por API key
- Rate limiting por tier
- Documentação Swagger automática
- Stack: Node.js + Fastify + MongoDB

### 4. Base de Dados

- **MongoDB** — flexível para dados heterogéneos de fontes diversas
- Cada fonte tem a sua própria collection
- Índices optimizados por caso de uso

## Princípios

1. **Cada serviço comunica apenas via HTTP** — sem acesso directo a BDs de outros serviços
2. **Consumer-Driven Contracts** — a API Pública define o formato que precisa; os conectores adaptam-se
3. **Dados primeiro** — melhor ter dados correctos e poucos do que muitos e inconsistentes
4. **Deploy independente** — cada serviço pode ser actualizado sem afectar os outros

## Stack

| Componente | Tecnologia |
|-----------|-----------|
| Runtime | Node.js 22 |
| Framework | Fastify 5 |
| Base de dados | MongoDB 7 |
| Deploy | Railway / Render |
| CI/CD | GitHub Actions |
| Docs | Swagger / Scalar |
