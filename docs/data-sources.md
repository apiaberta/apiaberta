# Data Sources

Inventário de fontes de dados públicos portugueses para conectores da API Aberta.

Legenda de estados: **live** = disponível na API | **planned** = no roadmap | **investigate** = a avaliar | **blocked** = acesso restrito

---

## ⛽ Energia

| Fonte | URL | Dados | Estado | Prioridade |
|-------|-----|-------|--------|------------|
| DGEG — Preços de Combustíveis | https://precoscombustiveis.dgeg.gov.pt/api/ | Preços por posto, tipo, distrito, marca (14 tipos) | **live** | — |
| DGEG — ArcGIS / SIG | https://sig.dgeg.gov.pt/ | Mapas de infraestrutura energética | investigate | baixa |
| ERSE — Regulação energética | https://www.erse.pt | Tarifas de eletricidade e gás | investigate | média |

---

## 🌤️ Meteorologia

| Fonte | URL | Dados | Estado | Prioridade |
|-------|-----|-------|--------|------------|
| IPMA — Previsão | https://api.ipma.pt/ | Previsões por local, avisos, temperatura, precipitação | **planned** | alta |
| IPMA — Observação | https://api.ipma.pt/ | Dados de estações meteorológicas (temperatura, vento, humidade) | **planned** | alta |
| IPMA — UV / Qualidade do ar | https://api.ipma.pt/ | Índice UV, partículas, ozono | investigate | média |

---

## 📊 Estatística Nacional

| Fonte | URL | Dados | Estado | Prioridade |
|-------|-----|-------|--------|------------|
| INE — Indicadores Económicos | https://www.ine.pt/ine/json_api/ | PIB, inflação, desemprego, população (registo gratuito) | **planned** | alta |
| INE — Censos | https://www.ine.pt | Dados dos censos 2021, distribuição populacional | investigate | média |
| Banco de Portugal | https://bpstat.bportugal.pt/api/ | Taxas de juro, câmbios, crédito, balança de pagamentos | investigate | média |
| PORDATA | https://www.pordata.pt | Séries históricas de indicadores sociais e económicos (sem API pública formal) | investigate | baixa |

---

## 📜 Legislação e Contratos Públicos

| Fonte | URL | Dados | Estado | Prioridade |
|-------|-----|-------|--------|------------|
| Portal BASE — Contratos | https://www.base.gov.pt/Base4/pt/docs/ | Contratos públicos, adjudicações, entidades (JSON/REST) | **planned** | alta |
| DRE — Diário da República | https://dre.pt/api/v2/ | Legislação, portarias, diplomas (JSON/REST) | **planned** | média |
| dados.gov.pt | https://dados.gov.pt/api/1/ | Portal open data oficial (CKAN API, múltiplos datasets) | investigate | média |

---

## 🚒 Protecção Civil e Emergências

| Fonte | URL | Dados | Estado | Prioridade |
|-------|-----|-------|--------|------------|
| ANEPC — Alertas e Ocorrências | https://www.bombeiros.pt/info-estatistica | Incêndios, inundações, alertas em tempo real | investigate | alta |
| ANEPC — Risco de Incêndio | https://www.anepc.pt | Índice de risco por distrito/município | investigate | alta |
| INEM — Estatísticas | https://www.inem.pt | Chamadas CODU, tempos de resposta (sem API pública conhecida) | investigate | média |

---

## 🏫 Educação

| Fonte | URL | Dados | Estado | Prioridade |
|-------|-----|-------|--------|------------|
| DGEEC — Estatísticas de Educação | https://www.dgeec.mec.pt | Alunos por escola, taxas de escolarização, resultados (sem API formal) | investigate | média |
| DGES — Ensino Superior | https://www.dges.gov.pt | Candidaturas, vagas, notas de entrada | investigate | média |
| ME — Carta Educativa | https://dados.gov.pt | Mapa de escolas, tipologias, localização | investigate | baixa |

---

## 🏥 Saúde

| Fonte | URL | Dados | Estado | Prioridade |
|-------|-----|-------|--------|------------|
| SNS — Tempos de Espera | https://www.sns.gov.pt | Urgências, cirurgias, consultas (dados publicados periodicamente) | investigate | média |
| DGS — Vigilância Epidemiológica | https://www.dgs.pt | Doenças de notificação obrigatória, gripe, COVID (histórico) | investigate | média |
| INFARMED — Medicamentos | https://www.infarmed.pt | Preços e disponibilidade de medicamentos | investigate | baixa |

---

## 🌿 Ambiente

| Fonte | URL | Dados | Estado | Prioridade |
|-------|-----|-------|--------|------------|
| APA — Qualidade do Ar | https://qualar.apambiente.pt/api | Índice de qualidade do ar por estação (JSON) | **planned** | alta |
| APA — Recursos Hídricos | https://snirh.apambiente.pt | Níveis de albufeiras, caudais, qualidade da água | investigate | média |
| ICNF — Biodiversidade | https://www.icnf.pt | Áreas protegidas, espécies, habitats | investigate | baixa |

---

## 🚌 Transportes

| Fonte | URL | Dados | Estado | Prioridade |
|-------|-----|-------|--------|------------|
| IMT — Veículos e Condutores | https://www.imt-ip.pt | Matrículas, cartas de condução, inspecções (sem API pública) | investigate | baixa |
| ANAC — Aviação Civil | https://www.anac.pt | Operações de voo, aeródromos, certificações | investigate | média |
| ANA Aeroportos | https://www.ana.pt | Tráfego de passageiros, voos em tempo real | investigate | média |
| CP — Comboios de Portugal | https://www.cp.pt | Horários, tarifas (sem API pública formal) | investigate | média |
| Carris/Metro Lisboa | — | GTFS realtime (em avaliação) | investigate | baixa |

---

## 🏢 Registos Empresariais e Judiciais

| Fonte | URL | Dados | Estado | Prioridade |
|-------|-----|-------|--------|------------|
| IRN / RNPC — Registo Nacional de Pessoas Colectivas | https://www.irn.mj.pt | NIPC, nome, sede, objecto social (sem API pública directa) | investigate | alta |
| Insolv — Insolvências | https://www.citius.mj.pt | Processos de insolvência publicados | investigate | baixa |
| ASAE — Fiscalização | https://www.asae.gov.pt | Estabelecimentos licenciados, fiscalizações | investigate | baixa |

---

## 🗳️ Eleições

| Fonte | URL | Dados | Estado | Prioridade |
|-------|-----|-------|--------|------------|
| SGMAI — Resultados Eleitorais | https://www.eleicoes.mai.gov.pt | Resultados históricos de todas as eleições por assembleia/freguesia | investigate | média |
| CNE | https://www.cne.pt | Recenseamento eleitoral, informação oficial | investigate | baixa |

---

## Como propor uma nova fonte

Abrir uma issue com a label `new-source` e incluir:
1. Nome da entidade
2. URL da API ou página de dados
3. Formato disponível (JSON, XML, CSV, SOAP…)
4. Frequência de actualização
5. Caso de uso (para que serve?)

---

*Última actualização: Fevereiro 2026*
