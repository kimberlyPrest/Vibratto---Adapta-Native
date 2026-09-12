# Evidência T3.10 — CA-3-028/029/030/031 (RED/GREEN por API)

Data: 2026-09-13 · Versões: v0.0.478–0.0.480 (QA verde) · Execução: provas por API no backend interno do preview.

## RED (rejeições corretas)

| Prova | Chamada | Resultado |
|---|---|---|
| R1 sem auth | GET /painel/direcao sem token | 401 |
| R2 papel inválido | GET /painel/ceo | 400 |
| R3 período invertido | ?periodo_inicio=2026-09-30&periodo_fim=2026-09-01 | 400 |
| R4 operator cria meta | POST metas_indicadores (operator) | bloqueado pela rule (400 — ocultação PB) |

## GREEN (comportamento correto)

| Prova | Chamada | Resultado |
|---|---|---|
| G1 painel direção | GET /painel/direcao | 15 KPIs em 4 grupos, com valor/anterior/variacao |
| G2 MRR conferido à mão | soma manual dos ganhos ativos | 8.336,11 = valor do painel ✓ |
| G3 comparativo duração igual | ?periodo_inicio=2026-09-01&periodo_fim=2026-09-15 | anterior 14d = atual 14d ✓ |
| G4 metas seed | GET /metas | 3 metas da CEO (2/mês, 1.5/sem, 4h/sem) |
| G5 admin edita meta | PATCH valor_meta 2→1 | painel reflete pct 300% atingida; restaurada para 2 |
| G6 recortes por papel | GET /painel/comercial e /controladoria | 6 KPIs cada |
| G7 administração | GET /painel/administracao | redireciona_para /dashboard |

## Premissa do MRR (decisão da CEO 13/09)

- BPO/Tesouraria/Controladoria: 12 meses renováveis automaticamente — valor = mensalidade, entra direto.
- Consultoria/CFO: prazo livre — só entra no MRR se recorrencia = mensal (campo novo, decidido no ganho).
- Negócios existentes: preenchidos como mensal (migration 0158).

## Limpeza

Migration 0159 — base final: 0 usuários de prova; 3 negócios reais; 3 metas seed (editáveis).
