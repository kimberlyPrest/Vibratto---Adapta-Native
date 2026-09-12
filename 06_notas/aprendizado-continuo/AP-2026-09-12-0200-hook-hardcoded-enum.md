# AP-2026-09-12-0200 — Hook de validação com enum hardcoded mascara mudança de schema

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: T3.01 / SPEC-3-000 (CA-3-001)
- Sinal 1: adicionar valor novo a um select via migration (0112/0113) não foi suficiente — o PATCH continuava 400 porque o hook `commercial_contract.js` (T2.01) valida `servico` contra lista SERVICES hardcoded em DOIS callbacks (create e update), independente do schema. Duas migrations "bem-sucedidas" não mudaram o comportamento; o diagnóstico só fechou lendo o hook.
- Evidência 1: logs de request do Skip (PATCH 400 "Failed to update record") + prova pós-fix (v0.0.378): PATCH servico=tesouraria 200, valor inválido 400.
- Regra 1: ao adicionar valor a enum validado por hook, procurar SEMPRE a lista duplicada nos hooks (grep por constantes inline) antes de escrever migration de schema; a lista do hook é a fonte real da validação, não o select do schema.
- Sinal 2: no JSVM do Skip, `field.set('values', arr)` em select NÃO persiste (migrations 0117/0119 aplicaram sem efeito observável); a atribuição direta `field.values = arr` persiste (0112/0120 — provado por PATCH 200).
- Evidência 2: contraste controlado — mesmo campo, mesmas provas por API, dois métodos de escrita diferentes.
- Regra 2: para alterar values de select em migration JSVM, usar atribuição direta `field.values = [...]` + app.save; nunca field.set('values', ...).
- Quando aplicar: qualquer campo select com validação server-side em hooks, e qualquer alteração de values de select via migration.
- Quando não aplicar: campos sem hook de validação e criação de campo novo (fields.add(new Field(...)) funciona — padrão 0110).
- Confiança: alta — causa raiz lida no código, efeito revertido e re-provado por API em ambos os casos.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto.
