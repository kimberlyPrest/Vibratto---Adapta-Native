# AP-2026-09-13-0920 — JSVM: função top-level não é visível em callback de cron

- Status: candidato
- Escopo: projeto do cliente (CRM Vibratto, runtime JSVM/PocketBase do Skip)
- Task/SPEC: T3.06 / SPEC-3-005
- Sinal: hook com função auxiliar top-level (`function executarAutomacoes()`) chamada dentro de `cronAdd` e `routerAdd` falhou no QA do Skip com "Hook scoping error: top-level declaration 'executarAutomacoes' is referenced inside a callback". A mesma regra já conhecida para routerAdd (AP-0200) se aplica a cronAdd — e o QA estático do Skip agora a detecta antes do deploy.
- Evidência: QA do skip_project_apply_changes v0.0.442 (integrations.ok=false com o erro acima); correção duplicando a lógica inline em cada callback (v0.0.443, QA verde).
- Regra reutilizável: em hooks JSVM do Skip, NENHUMA função/variável top-level pode ser referenciada dentro de callbacks (routerAdd, cronAdd, onRecord*) — duplicar a lógica inline em cada callback, mesmo que o arquivo cresça. O QA estático do Skip bloqueia o deploy quando detecta.
- Quando aplicar: todo hook JSVM novo com lógica compartilhada entre cron e rota.
- Quando não aplicar: não se aplica a migrations (rodam em contexto próprio).
- Confiança: alta — bloqueio explícito do QA e correção verificada.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto.
