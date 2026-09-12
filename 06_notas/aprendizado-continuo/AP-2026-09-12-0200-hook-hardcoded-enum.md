# AP-2026-09-12-0200 — Hook de validação com enum hardcoded mascara mudança de schema

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: T3.01 / SPEC-3-000 (CA-3-001)
- Sinal: adicionar valor novo a um select via migration (0112/0113) não foi suficiente — o PATCH continuava 400 porque o hook `commercial_contract.js` (T2.01) valida `servico` contra lista SERVICES hardcoded em DOIS callbacks (create e update), independente do schema. Duas migrations "bem-sucedidas" não mudaram o comportamento; o diagnóstico só fechou lendo o hook.
- Evidência: logs de request do Skip (PATCH 400 "Failed to update record" em 01:56–01:59) + prova pós-fix (v0.0.378): PATCH servico=tesouraria 200, valor inválido 400.
- Regra reutilizável: ao adicionar valor a enum validado por hook, procurar SEMPRE a lista duplicada nos hooks (grep por `SERVICES`/constantes inline) antes de escrever migration de schema; a lista do hook é a fonte real da validação, não o select do schema.
- Quando aplicar: qualquer campo select com validação server-side em hooks (origem, servico, status, prioridade, motivo_ganho...).
- Quando não aplicar: campos sem hook de validação — a migration de schema basta.
- Confiança: alta — causa raiz lida no código, efeito revertido e re-provado por API.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto.
