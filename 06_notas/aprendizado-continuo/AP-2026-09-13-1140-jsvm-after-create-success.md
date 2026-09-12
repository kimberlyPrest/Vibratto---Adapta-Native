# AP-2026-09-13-1140 — Model hook pré-save não executa save interno confiável

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: T3.08 / SPEC-3-007 (CA-3-024)
- Sinal: hook `onRecordCreate('tarefas')` com `$app.save()` de registro de OUTRA coleção (notificacoes) não criou a notificação em 3 provas — nem o save nem o catch executaram (0 eventos de debug em auditoria mesmo com log incondicional no início do callback). Substituído por `onRecordAfterCreateSuccess` → provou na 1ª tentativa.
- Evidência: evidencias/spec-3-007/ca-3-021-024-provas.md (G5); changelog 0.0.463–0.0.467.
- Regra reutilizável: em hooks JSVM de gravação, efeitos colaterais que salvam OUTRA coleção devem usar `onRecordAfterCreateSuccess` (pós-gravação) — o pré-save (`onRecordCreate`) não é confiável para save interno de outra coleção.
- Quando aplicar: criar registro dependente (notificação, auditoria enriquecida, contadores) a partir da criação de um registro.
- Quando não aplicar: validação/bloqueio de gravação (esse é papel de request hook ou API rule).
- Confiança: alta — 3 falhas consecutivas documentadas + 1 sucesso imediato após a troca.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto.
