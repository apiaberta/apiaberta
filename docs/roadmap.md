# Roadmap

## ✅ Fase 0 — Fundação (Completo)

Tudo o que está em produção hoje.

### Infra & Gateway
- [x] VPS provisionada (167.99.216.205) com nginx + PM2
- [x] Gateway Fastify (`@apiaberta/gateway`) em produção na :4000
- [x] Autenticação por API Key (`X-API-Key`)
- [x] Rate limiting por tier (Free / Pro / Admin)
- [x] Logging de uso em MongoDB
- [x] Proxy HTTP para serviços internos
- [x] Swagger docs em `/docs`
- [x] Healthcheck `GET /health`
- [x] Registo de developers `POST /v1/auth/register`

### Conector de Combustíveis
- [x] `connector-fuel` em produção na :3001
- [x] `GET /v1/fuel/prices` — médias nacionais por tipo
- [x] `GET /v1/fuel/stations` — postos com filtro por combustível, distrito, marca
- [x] `GET /v1/fuel/cheapest` — postos mais baratos
- [x] 14 tipos de combustível (DGEG)
- [x] Cron diário às 07:30 hora de Lisboa

### Presença pública
- [x] Landing page (apiaberta-landing)
- [x] Dev portal (apiaberta-app)
- [x] Status page pública
- [x] Servidor Discord da comunidade

---

## 🚀 Fase 1 — Novos Conectores (Q1–Q2 2026)

**Objectivo:** triplicar o número de datasets disponíveis.

### 1.1 — connector-ipma (Meteorologia)
- [ ] `GET /v1/weather/forecast` — previsão por município/local
- [ ] `GET /v1/weather/warnings` — avisos meteorológicos activos
- [ ] `GET /v1/weather/observations` — dados de estações em tempo real
- [ ] Cron a cada 30 min (dados mudam com frequência)
- [ ] Fonte: https://api.ipma.pt/ (API pública, sem registo)

### 1.2 — connector-base (Contratos Públicos)
- [ ] `GET /v1/contracts` — listagem paginada de contratos
- [ ] `GET /v1/contracts/:id` — detalhe de contrato
- [ ] `GET /v1/contracts/stats` — valor total, entidades top
- [ ] Filtros: entidade adjudicante, tipo, valor, data
- [ ] Fonte: https://www.base.gov.pt/Base4/pt/docs/ (JSON/REST)

### 1.3 — connector-ine (Estatísticas Nacionais)
- [ ] `GET /v1/statistics/indicators` — indicadores macro (PIB, inflação, desemprego)
- [ ] `GET /v1/statistics/population` — dados populacionais
- [ ] Actualização mensal / trimestral
- [ ] Fonte: https://www.ine.pt/ine/json_api/ (registo gratuito necessário)

### 1.4 — connector-apa (Qualidade do Ar)
- [ ] `GET /v1/environment/air-quality` — índice por estação/região
- [ ] `GET /v1/environment/air-quality/map` — visão agregada nacional
- [ ] Cron a cada hora
- [ ] Fonte: https://qualar.apambiente.pt/api (JSON)

---

## 📈 Fase 2 — Qualidade & Monetização (Q2–Q3 2026)

### 2.1 — Redis Cache
- [ ] Instalar Redis no VPS
- [ ] Cache de respostas por endpoint + query hash
- [ ] TTL configurável por tipo de dados (ex.: fuel = 1h, weather = 15 min)
- [ ] Invalidação automática após cada sync do conector
- [ ] Redução de carga no MongoDB em ~80%

### 2.2 — Tiers Pagos
- [ ] Definir preços (Free / Pro / Enterprise)
- [ ] Integração de pagamentos (Stripe)
- [ ] Dev portal com dashboard de uso e billing
- [ ] Invoicing automático
- [ ] SLA 99.9% documentado e monitorizado

### 2.3 — Webhook Notifications
- [ ] `POST /v1/webhooks` — registar endpoint para eventos
- [ ] Eventos: `fuel.prices.updated`, `weather.warning.issued`, `contract.published`
- [ ] Retry com backoff exponencial
- [ ] Interface no dev portal para gerir webhooks

### 2.4 — Qualidade & Observabilidade
- [ ] Testes automatizados (Vitest) em todos os conectores
- [ ] CI/CD GitHub Actions com deploy automático
- [ ] Alertas quando um conector falha (Discord + email)
- [ ] Métricas de uptime públicas em status.apiaberta.pt

---

## 🌍 Fase 3 — Escala & Adopção (H2 2026+)

### 3.1 — SDK Oficial
- [ ] `@apiaberta/sdk` — JavaScript/TypeScript
- [ ] `apiaberta` — Python
- [ ] Exemplos de código para casos de uso comuns
- [ ] Publicar em npm e PyPI

### 3.2 — Mais Conectores
- [ ] ANEPC — alertas de protecção civil e risco de incêndio
- [ ] DRE — legislação e diplomas
- [ ] SGMAI — resultados eleitorais históricos
- [ ] IRN/RNPC — pesquisa de pessoas colectivas
- [ ] Banco de Portugal — indicadores financeiros

### 3.3 — Adopção Institucional
- [ ] Contactar startups e PMEs que possam beneficiar
- [ ] Apresentar à AMA (Agência para a Modernização Administrativa)
- [ ] Parceria com universidades (datasets para investigação)
- [ ] Programa de "API Aberta Verified" para integradores

---

## ❌ Fora de âmbito (para já)

- Dados sensíveis (registos clínicos, finanças pessoais)
- Autenticação de cidadãos (Chave Móvel Digital)
- Integração com sistemas internos governamentais
- Dados em tempo real que exijam acordos especiais
