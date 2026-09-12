# Evidência T3.12 — Teste humano executado (a pedido da CEO, 2026-09-13 13:09)

Execução: Deni.Ai no backend interno do preview, com a ficha REAL da Felicidade Collective.
Versão testada: v0.0.504 (QA verde).

## Roteiro executado

| # | Teste | Resultado |
|---|---|---|
| T1 | Rodar o motor | ✅ 18 obrigações geradas + 4 atrasadas marcadas + 4 exceções E10 abertas |
| T2 | Lista do dia | ✅ 18 obrigações ordenadas por prazo, fluxo completo (coleta → lançamento → projeção → autorização → banco), ciclo semanal de contas a pagar (11/09 e 18/09), faturamento dias 22-25/09 e 06-09/10, conciliações 18/09 e 25/09 |
| T3 | Baixa em 1 toque | ✅ 200, status concluida |
| T4 | Baixa em lote | ✅ 3/3 baixadas, 0 erros |
| T5 | Exceção E10 resolvida pela baixa | ✅ exceção da coleta_canal virou "resolvida"; as 3 restantes continuam abertas (correto) |
| T6 | Bloqueio com motivo | ✅ 200, status bloqueada |
| T7 | Baixa em bloqueada | ✅ 400 com mensagem clara |
| T8 | Auditoria do motor | ⚠️→✅ FALHOU na 1ª rodada — causa: coleção auditoria só aceitava acao create/update (migration 0010); eventos do motor eram descartados silenciosamente no try/catch. FIX migration 0164 (novos valores: motor_executado, baixa, baixa_lote, bloqueio, delete) + registro_id não vazio. Re-teste: motor_executado e baixa registrados corretamente |
| T9 | Regressão | ✅ meu-dia 200, ficha-operacional 200 |
| T10 | Auditoria de baixa | ✅ prevista → concluida registrada |

## Estado final (preservado para uso real)

- 13 obrigações: 12 previstas + 1 bloqueada (com motivo real de negócio)
- 4 exceções: 3 abertas (obrigações atrasadas de 11/09) + 1 resolvida
- Ficha real da Felicidade intacta; dedup provado (2ª execução: 0 geradas / 18 ignoradas)

## Bug encontrado e corrigido no teste

**Auditoria silenciosamente falhando** — os eventos do motor (motor_executado, baixa, baixa_lote, bloqueio) violavam o select restrito da coleção auditoria (só create/update desde a migration 0010) e o try/catch engolia o erro. Corrigido na migration 0164 + registro_id não vazio. Lição: try/catch de auditoria deve logar o erro, não engolir — registrado como melhoria para o próximo ciclo.
