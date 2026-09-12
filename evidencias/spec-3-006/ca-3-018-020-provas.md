# Evidência T3.07 — CA-3-018/019/020 (RED/GREEN por API)

Data: 2026-09-13 · Versões: v0.0.448–0.0.451 (QA verde em todas) · Execução: provas por API no backend interno do preview.

## RED (rejeições corretas)

| Prova | Chamada | Resultado |
|---|---|---|
| R1 sem consentimento | POST /entrada/publico sem consentimento_lgpd | 400 |
| R2 tempo <20s | POST com tempo_segundos=2 | 400 |
| R3 CNPJ inválido | POST com cnpj="123" | 400 |
| R4 lista sem auth | GET /entrada/leads sem token | 401 |
| R5 vincular sem auth | POST /entrada/leads/{id}/vincular sem token | 401 |
| R6 re-vincular | POST vincular em lead já vinculado | 400 "já está vinculado" |

## GREEN (comportamento correto)

| Prova | Chamada | Resultado |
|---|---|---|
| G1 envio válido | POST /entrada/publico (decisor, 2–10m, imediata, sonho) | 200 `{ok, temperatura:"quente"}` — score 92 confirmado na lista |
| G2 lista interna | GET /entrada/leads?temperatura=quente (auth) | 200 com lead e campos estruturados |
| G3 vincular | POST /entrada/leads/{id}/vincular | 200 `{contato, negocio}` — oportunidade criada |
| G4 rate limit | 3 envios + 1 do mesmo IP | 200 200 200 **429** (após fix de IP) |
| G5 honeypot | POST com website="spam.com" | 200 silencioso, nenhum registro criado |

## Fixes provados no caminho

1. v0.0.449: rate limit não aplicava — IP chegava vazio (proxy). Fix: X-Forwarded-For. Prova: ainda 200 no 4º (header API errada).
2. v0.0.450: `e.request.getHeader` não existe no JSVM → `e.request.header.get('X-Forwarded-For')` + Cf-Connecting-Ip. Prova: 429 no 4º envio (G4).

## Limpeza

Migrations 0150/0151/0152 — base final verificada por API: 0 leads_entrada, 0 contatos de prova, 0 negócios de prova.

## Pendências para o teste humano

- Dedup (G-dedup): reenvio do mesmo e-mail com negócio aberto <90 dias → `dedup:true` + evento na oportunidade (bloqueado pelo rate limit durante as provas automatizadas — mesmo IP).
- UI `/entrada` no celular: preenchimento completo, máscara de CNPJ, enriquecimento, confirmação.
- Botão "Vincular" na lista interna.
