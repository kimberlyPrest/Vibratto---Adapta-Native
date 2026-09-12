# AP-2026-09-13-0810 — JSVM: query string via e.request.url.query(), não e.request.query()

- Status: candidato
- Escopo: projeto do cliente (CRM Vibratto, runtime JSVM/PocketBase do Skip)
- Task/SPEC: T3.03 / SPEC-3-002
- Sinal: rota GET registrada com `e.request.query().get('negocio')` falhou com `TypeError: Object has no member 'query'` (status 0 nos logs de request do Skip); a mesma rota com `e.request.url.query().get('negocio')` respondeu 200.
- Evidência: logs do Skip (mcp_skip_skip_cloud_list_logs, source=requests, 2026-09-12 03:41Z — 4 entradas com o TypeError); v0.0.436 QA verde; provas GREEN por API após o fix.
- Regra reutilizável: em hooks JSVM do Skip, query string se lê com `e.request.url.query().get('<param>')` — `e.request.query()` não existe. Complementa a lição anterior (`e.request.query()` NÃO existe — usar `e.request.url.query().get(...)`), agora com causa raiz confirmada por log.
- Quando aplicar: qualquer endpoint GET com parâmetro de query em hooks JSVM.
- Quando não aplicar: não se aplica a rotas com path param (`e.request.pathValue`) nem a body (`e.requestInfo().body`).
- Confiança: alta — erro reproduzido nos logs, fix verificado por API e QA verde.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto.
