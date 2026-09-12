# Changelog — CRM Vibratto

## [0.0.437] — 2026-09-13 — T3.03 WhatsApp P1 implementada (aguardando teste humano)

### Adicionado (T3.03 — WhatsApp P1, SPEC-3-002, doc Onda 3 §14)
- **Coleção `interacoes_whatsapp`** (migration 0142): negócio (relation obrigatória), contato (relation opcional), direção (entrada/saida), resumo (5–5000 chars), resultado (sem_resposta/resposta/reuniao_agendada/proposta_solicitada/negativo), responsável, próxima ação (descrição + data), trilha json. Delete bloqueado (deleteRule null) — append-only.
- **Hook `whatsapp_interacoes.js`** — 2 rotas: POST `/backend/v1/whatsapp/interacoes` (auth: valida negócio existe/não arquivado, resumo, resultado, direção; auditoria com snapshot mínimo; próxima ação futura atualiza a oportunidade — só contexto, nunca campos comerciais; save de sistema não dispara request hooks, guard T2.18 intacto) e GET `/backend/v1/whatsapp/interacoes?negocio=X` (lista -created com nomes expandidos).
- **UI `WhatsAppNegocio`** — botão "WhatsApp" no menu Mais ⌄ da oportunidade: registro (direção, resultado, resumo, próxima ação) + lista das interações.
- **Consulta 360º** — novo bloco `whatsapp` (total, última interação, próxima ação via WhatsApp) no endpoint e na UI.

### Corrigido no caminho
- GET quebrava: `e.request.query()` não existe no JSVM — correto é `e.request.url.query().get(...)` (v0.0.436; diagnóstico via logs do Skip).

### Provas (CA-3-006 a CA-3-008)
- RED: 401 sem auth · 404 negócio inexistente · 400 resumo curto · 400 resultado inválido · 400 direção inválida · 400 GET sem negócio · 403 delete.
- GREEN: POST 200 (com e sem próxima ação; `proxima_acao_atualizada: true` e oportunidade refletindo) · GET 200 lista ordenada · 360º com bloco whatsapp.
- Limpeza (0143): fixtures de prova removidas, próxima ação original da Felicidade restaurada; caso real registrado (1 interação, reunião_agendada) para o teste humano.
- QA verde v0.0.435→0.0.437.

### Pendência de verificação
- Abertura do modal WhatsApp via clique no menu não confirmada no teste automatizado de browser (item do menu presente e clicável; modal não renderizou no snapshot) — verificar no teste humano.

## [0.0.435] — 2026-09-13 — Governança da leva 3 (WhatsApp P1)

### Adicionado
- **SPEC-3-002** — WhatsApp P1: registro estruturado de interações WhatsApp na oportunidade (coleção `interacoes_whatsapp` append-only, endpoint POST/GET server-side, UI no menu Mais ⌄, bloco WhatsApp na consulta 360º, auditoria). Critérios CA-3-006 a CA-3-008 com provas TDD.
- **fase.md** — T3.03 detalhada (leva 3).
- Estado: T3.03 em `aguardando_autorizacao`.

## [0.0.434] — 2026-09-13 — T3.02b implementada (aguardando teste humano)

### Adicionado (T3.02b — ficha de preparação da proposta, doc Onda 3 §10)
- **Coleção `fichas_proposta`** (migration 0136): negócio (relation), versão, 7 campos editáveis do time (solução recomendada, escopo sugerido, frequência, senioridade, entregáveis, premissas de precificação, pontos a confirmar), motivo de atualização. Delete bloqueado (deleteRule null).
- **API rule do update** (migration 0137): `@request.body.motivo_atualizacao != ''` — PATCH sem motivo bloqueado.
- **Hook `ficha_proposta_rules.js`**: motivo obrigatório (mín. 10 caracteres) no create e update.
- **Endpoint `GET /backend/v1/fichas/{negocioId}`**: consolida em uma leitura — oportunidade (contato/empresa/serviço/origem/próxima ação), qualificação (percentual + respostas legíveis), diagnóstico atual, formulário respondido (resumo) e ficha editável mais recente + **completude explícita** (o que falta antes da proposta, nada escondido).
- **UI `FichaPropostaNegocio`**: botão "Ficha da proposta" no menu Mais ⌄; blocos de consolidação + editor da leitura do time com versionamento (v1 → v2 → ...) e motivo obrigatório.
- Provas API: 401 sem auth · 404 negócio inexistente · 200 consolidação · 400 sem motivo · 200 create/update com motivo · bloqueio de update sem motivo · 403 delete. Evidência: artifacts/T302b_evidencia.md.

### Lições (candidata a AP)
- JSVM hooks (request e model) não expõem before confiável no update neste runtime (v2–v7 falharam) — validação de update resolvida com API rule da coleção. Gramática PB: sem ternário, sem length().

## [0.0.418] — 2026-09-13 — T3.02 CONCLUÍDA (teste humano aprovado)

### Adicionado (T3.02 — formulários inteligentes, SPEC-3-001)
- **Coleção `formularios`** (migration 0130): token único 48 chars, negócio/contato/empresa, solução (bpo_financeiro/cfo_as_a_service/consultoria), status (gerado/enviado/respondido/expirado), respostas JSON, resumo, consentimento LGPD + versão, trilha append-only. Create/update/delete apenas server-side (null rules).
- **Hook `formularios_inteligentes.js`** — 4 rotas: POST gerar (auth), POST enviar/{id} (auth), GET/POST publico/{token} (público). Resposta única (409 em reenvio), consentimento LGPD obrigatório (400 sem), atualização automática da oportunidade com contexto estruturado (perfil/volume/complexidade/dores/objetivos) — nunca campos comerciais. Auditoria em toda transição.
- **UI interna** — botão "Formulário" no menu Mais ⌄ da oportunidade: gerar link (copia automático), marcar enviado, ver resumo da resposta.
- **Página pública** `/formulario/:token` (mobile-first, sem login): BPO 16 campos, CFO 11, Consultoria 10 (doc Onda 3 §6-8) + LGPD obrigatório.
- **Campos de contexto na oportunidade** (0130): formulario_status, dados_formulario (json), formulario_resumo.

### Corrigido
- Botão "Mais ⌄" do card não abria o menu — listener global de clique fechava o menu no mesmo evento; fix com stopPropagation (v0.0.415, debug-2026-09-12-t302-botao-mais.md).

### Provas (CA-3-002 a CA-3-005)
- RED/GREEN por API: 401 sem auth · 400 solução inválida · 200 gerar (token 48) · 200 leitura pública · 400 sem LGPD · 200 resposta com LGPD · 409 reenvio · 404 token inválido · oportunidade atualizada (status+resumo+dados_formulario) · 403 delete · 200 enviar · 3 eventos de auditoria.
- QA verde v0.0.408→0.0.418. Evidência: artifacts/T302_evidencia_ca3002.md (workspace).

### Teste humano
- Aprovado pela CEO em 12/09 23:55 ("funcionou") após correção do botão Mais.

### Pendências
- Delete de registros via migration ($app.delete) não removeu fixtures — resolvido por invalidação (status expirado); investigar causa raiz do delete em debug futuro.
- Card "Contas & Empresas" sem página própria (Fase 3).
- Corrigir contato "ROMEU" → maiúscula.

## [0.0.408] — 2026-09-12 — Governança do ciclo T3.01-pós + SPEC-3-001 (T3.02)

### Adicionado
- **SPEC-3-001** — formulários inteligentes por solução (BPO/CFO/Consultoria): coleção `formularios` com token público, 3 formulários (15/9/8 campos do doc Onda 3), atualização automática da oportunidade, consentimento LGPD, auditoria. Critérios CA-3-002 a CA-3-005 com provas TDD.
- **fase.md** — T3.02 detalhada (critérios, provas, evidência) + T3.02b (ficha de preparação da proposta) separada para recorte mínimo.
- **AP-2026-09-12-0230** — aprendizado: atribuição direta `field.values = [...]` confirmada em 3ª aplicação (0128/0129).
- Estado: T3.02 em `aguardando_autorizacao`.

### Teste humano
- Canal Comunidade aprovado pela CEO em 12/09 23:31 ("Perfeito, teste realizado") — ciclo T3.01-pós fechado.
