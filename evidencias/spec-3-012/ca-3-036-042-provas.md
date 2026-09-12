# Evidência T3.12 — CA-3-036 a CA-3-042 (RED/GREEN por API)

Data: 2026-09-13 · Versões: v0.0.495–0.0.502 (QA verde) · Execução: provas por API no backend interno.

## RED (rejeições corretas)

| Prova | Chamada | Resultado |
|---|---|---|
| R1 sem auth | GET /obrigacoes sem token | 401 |
| R2 operator no motor | POST /obrigacoes/gerar (operator) | 403 |
| R3 baixa inexistente | POST /obrigacoes/xxx/baixa | 404 |
| R4 baixa duplicada | POST baixa em obrigação já concluída | 400 |
| R5 bloqueio sem motivo | POST bloquear sem motivo_bloqueio | 400 |
| R6 create manual (CA-3-042) | POST /api/collections/obrigacoes/records | 403 (createRule null) |
| R7 delete | DELETE obrigação | 403 |

## GREEN (comportamento correto)

| Prova | Critério | Resultado |
|---|---|---|
| G1 motor | CA-3-036 | 18 obrigações geradas da ficha real (Felicidade: contas a pagar semanal/segunda, faturamento dias 10/25, conciliação semanal) — fluxo com offsets por tipo (coleta -3d ... cadastro banco dia D) |
| G2 dedup | CA-3-040 | 2ª execução: geradas 0, ignoradas 18 |
| G3 atraso→exceção | CA-3-037 | 4 obrigações vencidas → atrasadas + 4 exceções E10 abertas (dedup por obrigação) |
| G4 baixa 1 toque | CA-3-038 | 200, status concluida, data_conclusao + concluida_por |
| G5 baixa lote | CA-3-038 | 2/2 baixadas, 0 erros |
| G6 bloqueio | — | 200 com motivo; bloqueada não aceita baixa (400) |
| G7 reserva | CA-3-039 | titular inativo → obrigações nascem para o reserva com substituicao_aplicada=true |
| G8 suspensão | CA-3-041 | ficha suspensa → 0 novas gerações; histórico preservado |

## Limpeza

Migration 0163 — base final: 0 obrigações/exceções de prova; ficha de prova AG removida; usuário inativo removido; ficha real da Felicidade preservada; 3 negócios reais.
