# 🇵🇹 API Aberta

**Uma API unificada, moderna e aberta para dados públicos portugueses.**

A API Aberta agrega e normaliza dados dispersos do governo português numa única interface REST coerente — com documentação clara, API keys gratuitas e endpoints estáveis.

[![Status](https://img.shields.io/badge/status-em%20construção-yellow)](https://github.com/apiaberta)
[![Stack](https://img.shields.io/badge/stack-Node%20%7C%20Fastify%20%7C%20MongoDB-green)](https://github.com/apiaberta/api)
[![Licença](https://img.shields.io/badge/licença-MIT-blue)](LICENSE)

---

## O problema

As APIs do governo português são:
- **Fragmentadas** — cada entidade tem a sua própria API (ou não tem nenhuma)
- **Inconsistentes** — formatos diferentes, autenticação diferente, documentação inexistente
- **Instáveis** — SOAP, XML, WSDL de 2003, downtimes frequentes
- **Lentas** — latências de 2–5 segundos são comuns

## A solução

A API Aberta funciona como uma camada de abstracção:

```
Fonte Gov → Conector → Ingestão → Base de Dados → API Pública
 (DGEG, INE,   (normaliza,   (valida,      (MongoDB)    (REST/JSON,
  DRE, Base)    extrai)       deduplica)                  API keys)
```

- **Dados normalizados** em JSON moderno
- **Endpoints estáveis** independentes das fontes originais
- **Latência baixa** — os dados são pré-processados e armazenados
- **API keys gratuitas** com rate limiting generoso
- **Documentação interactiva** via Swagger

---

## Fontes de dados

| Fonte | Descrição | Estado |
|-------|-----------|--------|
| [DGEG](https://www.dgeg.gov.pt) | Preços de combustíveis | 🔄 Em desenvolvimento |
| [Portal Base](https://www.base.gov.pt) | Contratos públicos | 📋 Planeado |
| [INE](https://www.ine.pt) | Estatísticas nacionais | 📋 Planeado |
| [DRE](https://dre.pt) | Diário da República | 📋 Planeado |
| [IPMA](https://www.ipma.pt) | Meteorologia | 📋 Planeado |

---

## Estrutura do projecto

```
apiaberta/
├── api/          → API Fastify (endpoints públicos, auth, rate limiting)
├── connectors/   → Conectores por fonte de dados
├── ingest/       → Serviço de validação e normalização
└── docs/         → Documentação técnica
```

Cada componente é um serviço independente com o seu próprio repositório.

---

## Roadmap

Ver [docs/roadmap.md](docs/roadmap.md) para o plano detalhado.

**Fase 1 — MVP (Março 2026)**
- [ ] Conector DGEG (preços combustíveis)
- [ ] API Fastify com endpoint `/v1/fuel/prices`
- [ ] Sistema de API keys
- [ ] Documentação Swagger

**Fase 2 — Expansão (Abril–Junho 2026)**
- [ ] Portal de registo de developers
- [ ] 3+ conectores adicionais
- [ ] Rate limiting por tier
- [ ] Dashboard de utilização

---

## Contribuir

Toda a ajuda é bem-vinda — especialmente conectores para novas fontes de dados.

Ver [CONTRIBUTING.md](CONTRIBUTING.md) para começar.

---

## Licença

MIT — livre para usar, modificar e distribuir.
