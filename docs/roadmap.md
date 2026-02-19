# Roadmap

## Fase 1 — MVP (Março 2026)

Objectivo: ter um endpoint público funcional com dados reais.

### Milestone 1.1 — Estrutura base
- [ ] Repositórios criados (`api`, `connectors`, `ingest`)
- [ ] Setup Node + Fastify + MongoDB
- [ ] Healthcheck endpoint (`GET /health`)
- [ ] Deploy básico no Railway

### Milestone 1.2 — Primeiro conector
- [ ] Conector DGEG — preços de combustíveis
- [ ] Cron job de actualização (diário)
- [ ] Dados a fluir para MongoDB

### Milestone 1.3 — API pública v1
- [ ] `GET /v1/fuel/prices` — preços médios nacionais
- [ ] `GET /v1/fuel/stations` — postos por localização
- [ ] Documentação Swagger
- [ ] API keys (registo simples por email)

---

## Fase 2 — Crescimento (Abril–Junho 2026)

### Milestone 2.1 — Portal de developers
- [ ] Página de registo de API key
- [ ] Dashboard de utilização
- [ ] Rate limiting por tier (Free / Pro)

### Milestone 2.2 — Mais conectores
- [ ] Portal Base — contratos públicos
- [ ] INE — indicadores económicos
- [ ] DRE — legislação recente

### Milestone 2.3 — Qualidade
- [ ] Testes automáticos (CI/CD)
- [ ] Alertas de falha nos conectores
- [ ] SLA público (uptime page)

---

## Fase 3 — Escala (2º semestre 2026)

- [ ] Abrir contribuições externas para novos conectores
- [ ] API programática (Chave Móvel Digital)
- [ ] SDKs (JavaScript, Python)
- [ ] Versão paga com SLA garantido

---

## O que NÃO está no scope (por agora)

- Dados sensíveis (saúde, finanças pessoais)
- Autenticação de cidadãos
- Integração com sistemas internos do governo
