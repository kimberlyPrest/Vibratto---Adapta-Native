# Changelog — CRM Vibratto

## [0.0.417] — 2026-09-13 — T3.02 CONCLUÍDA (teste humano aprovado)

### Adicionado (T3.02 — formulários inteligentes, SPEC-3-001)
- Coleção `formularios` (migration 0130): token único de 48 chars, vínculos contato/empresa/oportunidade, respostas JSON, resumo, consentimento LGPD, trilha append-only. Regras: leitura autenticada; create/update/delete somente server-side.
- Hook `formularios_inteligentes.js`: POST /backend/v1/formularios/gerar (auth), POST /backend/v1/formularios/enviar/{id} (auth), GET/POST /backend/v1/formularios/publico/{token} (público, resposta única, LGPD obrigatório).
- Campos de contexto na oportunidade: `formulario_status`, `dados_formulario`, `formulario_resumo` — atualização automática ao responder (perfil, volume, complexidade, dores, objetivos, urgência; NUNCA campos comerciais).
- UI: `FormularioNegocio` (gerar link, copiar, marcar enviado, ver resumo) + página pública `/formulario/:token` (3 formulários: BPO 16 campos, CFO 11, Consultoria 10, mobile-first).
- Provas API: 401 sem auth, 400 solução inválida/sem LGPD, 409 reenvio, 404 token inválido, 200 gerar/ler/responder/enviar, delete 403, auditoria 3 eventos. Evidência: artifacts/T302_evidencia_ca3002.md.

### Corrigido
- **DEBUG T3.02**: botão "Mais ⌄" não abria — listener global de clique fechava o menu no mesmo evento; fix `stopPropagation` (v0.0.415), verificado no browser real (menu com 4 itens). Debug: 06_notas/debug/debug-2026-09-12-t302-botao-mais.md.
- Limpeza de fixtures de prova: migrations 0131–0134 falharam silenciosamente; 0135 com `app.delete(record)` removeu as 2 fixtures (base limpa: 0 formulários, 3 negócios reais).

### Lições (AP-2026-09-13-0040)
- JSVM: em migration, delete de registro é `app.delete(rec)` no app da migration — `$app.delete(rec)` não remove (falha silenciosa em try/catch).
- Listener global de clique fecha menus no mesmo evento do botão — usar `stopPropagation` no botão que abre.
- Rotas: `/{id}/enviar` conflita com `/publico/{token}` no routescan — usar `/enviar/{id}`.

## [0.0.408] — 2026-09-12 — Governança do ciclo T3.01-pós + SPEC-3-001 (T3.02)

### Adicionado
- **SPEC-3-001** — formulários inteligentes por solução (BPO/CFO/Consultoria): coleção `formularios` com token público, 3 formulários (15/9/8 campos do doc Onda 3), atualização automática da oportunidade, consentimento LGPD, auditoria. Critérios CA-3-002 a CA-3-005 com provas TDD.
- **fase.md** — T3.02 detalhada (critérios, provas, evidência) + T3.02b (ficha de preparação da proposta) separada para recorte mínimo.
- **AP-2026-09-12-0230** — aprendizado: atribuição direta `field.values = [...]` confirmada em 3ª aplicação (0128/0129).
- Estado: T3.02 em `aguardando_autorizacao`.

### Teste humano
- Canal Comunidade aprovado pela CEO em 12/09 23:31 ("Perfeito, teste realizado") — ciclo T3.01-pós fechado.
