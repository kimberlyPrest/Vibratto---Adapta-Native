# Estado atual — Adapta Cliente

- task_id: T3.06 (Automações Se/Então — §12 do doc Onda 3)
- champion: Deni.Ai
- spec: SPEC-3-005-automacoes-se-entao.md (base: doc Onda 3 §12)
- etapa: aguardando_teste_humano
- criterio: CA-3-015 execuções corretas e idempotentes; CA-3-016 alertas de saúde; CA-3-017 somente leitura comercial + leitura estruturada
- autorizacao_implementacao: confirmada — 2026-09-13 09:00, owner: "sim"
- teste_humano: pendente
- verificacao_automatica: passou — QA verde v0.0.443–0.0.447; provas API (RED 401/403/400; GREEN: fixture com proposta emitida há 5 dias gerou follow_up_proposta com detalhe correto; reexecução idempotente — 1 registro total; proposta com decisão não gera execução; create direto na coleção 403; regressão comercial confirmada — valor/estágio/status dos 3 negócios reais idênticos antes/depois); UI verificada no browser real: seção "Automações de hoje" com 4 cards e a execução da fixture visível (print artifacts/t306_automacoes_painel.png); limpeza 0148 (base final: 0 execuções, 3 negócios reais)
- aprendizado: pendente
- ultima_acao: implementação da T3.06 concluída (v0.0.447) — provas API verdes, UI no painel verificada, base limpa
- proxima_acao: aguardar teste humano (roteiro entregue)
- atualizado_em: 2026-09-13T09:20:00-03:00
