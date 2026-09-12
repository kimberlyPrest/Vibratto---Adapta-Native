# Changelog — CRM Vibratto

## [0.0.459] — 2026-09-13 — D2+D5 implementadas e provadas (correção autorizada pela CEO)

- 2026-09-13 · [Deni.Ai] · Correções D2/D5 da T3.07 (autorização da CEO 10:34 — "PODE IMPLEMENTAR"). **D2**: relato opcional com mínimo 30 chars — validação server-side no hook leads_entrada.js (400 com mensagem clara) + contador orientador no UI /entrada (placeholder com exemplo, contador âmbar abaixo de 30, ✓ ao atingir). **D5**: retenção 24 meses — hook leads_entrada_retencao.js com cron diário 03:00 (padrão audit_retention.js) + execução manual admin-only POST /backend/v1/entrada/retencao/executar (padrão T3.06); leads `novo` eliminados 24 meses após coleta ou último contato (trilha); delete via $app.delete em contexto sistema (deleteRule null bloqueia só a API — provado 403).
- Provas: D2 RED (10 chars → 400) + GREEN (43 chars → 200; sem relato → 200). D5 RED (sem auth 401) + GREEN funcional (fixture created retroativo 2024-08-01 via SQL em 0153 → execução manual removidos:1; leads reais intactos 3/3). Limpeza 0154 — base final 0 provas, 3 leads reais, rate limit restaurado para 3.
- Fixes: AP-0920 reincidente (constante top-level em callback de cron → inline, v0.0.456); rate limit elevado temporariamente para 100 durante a prova GREEN D2 e restaurado.
- QA verde v0.0.455–0.0.459.

## [0.0.459] — 2026-09-13 — D2+D5 implementadas (decisões da CEO de 13/09)

- 2026-09-13 · [Deni.Ai] · Correção T3.07 autorizada pela CEO ("PODE IMPLEMENTAR"): **D2 — relato livre** opcional com mínimo de 30 caracteres se preenchido — validação server-side no hook `leads_entrada.js` (400 com mensagem clara) + contador orientador no UI `/entrada` (placeholder com exemplo, contador "X caracteres restantes" em âmbar abaixo de 30, ✓ verde ao atingir). **D5 — retenção**: hook `leads_entrada_retencao.js` com cron diário 03:00 (padrão audit_retention.js) + endpoint de execução manual admin-only `POST /backend/v1/entrada/retencao/executar` (padrão T3.06); política: leads `novo` (nunca vinculados) eliminados 24 meses após coleta ou último contato (trilha), o que for mais recente; delete via $app.delete (contexto sistema — deleteRule null bloqueia só a API, provado 403).
- Provas: D2 RED (relato 10 chars → 400) + GREEN (43 chars → 200; sem relato → 200). D5 RED (sem auth 401) + GREEN funcional (fixture com created retroativo 2024-08-01 via SQL em 0153 → execução manual removidos:1, detalhes com token e último contato; leads reais intactos 3/3). Limpeza 0154 (SQL DELETE prova-d2-%) — base final 0 provas, 3 leads reais, rate limit restaurado para 3 (regressão ok).
- Fixes no caminho: AP-0920 reincidente (RETENCAO_MESES top-level → inline no callback, v0.0.456); rate limit elevado temporariamente para 100 durante a prova GREEN D2 (janela 1h esgotada pelo IP do sandbox) e restaurado para 3.
- QA verde v0.0.455–0.0.459.

## [0.0.452] — 2026-09-13 — T3.07 CONCLUÍDA (teste humano aprovado pela CEO)

- 2026-09-13 · [Deni.Ai] · Task T3.07 concluída: Porta 1 — formulário público de entrada (SPEC-3-006, CA-3-018/019/020). Formulário `/entrada` sem login (90–120s, mobile-first, 3 blocos: identificação + qualificação + roteamento por sintoma), coleção `leads_entrada` append-only (0149), score 0–92 server-side com temperatura (quente ≥60 / morno 35–59 / frio <35), dedup por e-mail (negócio aberto <90 dias), rate limit por IP/hora (429 provado), honeypot silencioso, tempo mínimo 20s, UTM + origem declarada, LGPD duplo com versão LGPD-V1-2026-09, enriquecimento BrasilAPI informativo (falha não bloqueia), endpoint de vincular (contato + oportunidade saudável, re-vinculação 400). Provas RED 6 / GREEN 5 por API em evidencias/spec-3-006/; revalidação do zero na conclusão (401 sem auth; 400 sem consentimento/dor). Teste humano aprovado pela CEO em 2026-09-13 10:06 — "muito bom, validado!" (UI completa no celular: envio, LGPD, campos, confirmação). Limpeza 0150–0152 verificada (base 0 provas). Governança: fase.md T3.06/T3.07 ✅ (corrige linha T3.06 que ainda constava como aguardando teste), STATUS 8/N, controle.md + AP-2026-09-13-1006-jsvm-header-ip.md (leitura de header HTTP em request hook JSVM: e.request.header.get, não getHeader; provar por API antes de depender em regra de segurança).
- Aprendizado: AP-2026-09-13-1006-jsvm-header-ip.md (header HTTP em request hook JSVM).

## [0.0.451] — 2026-09-13 — T3.07 Porta 1 implementada (CA-3-018/019/020, aguardando teste humano)

### Adicionado

- **Formulário público de entrada (Porta 1)**: página `/entrada` (sem login, 90–120s, mobile-first) em 3 blocos — identificação (nome, e-mail, WhatsApp, decisor, CNPJ com máscara + enriquecimento BrasilAPI informativo, falha NÃO bloqueia), qualificação (faturamento, CNPJs do grupo, colaboradores, regime, ERP, quem cuida do financeiro) e roteamento por sintoma (dor principal, dores secundárias, relato, urgência, sonho 12 meses).
- **Coleção `leads_entrada`** (migration 0149, append-only — delete bloqueado): token, contato, qualificação, score (0–92 server-side), temperatura (quente ≥60 / morno 35–59 / frio <35), UTM (json), origem declarada, IP, LGPD duplo (consentimento obrigatório + opt-in marketing opcional), vínculo à oportunidade no dedup.
- **Hook `leads_entrada.js`**: `GET/POST /backend/v1/entrada/publico` (público), `GET /backend/v1/entrada/leads` (auth, filtro por temperatura), `POST /backend/v1/entrada/leads/{id}/vincular` (auth — cria contato dedup por e-mail + oportunidade saudável na primeira etapa ativa, responsável = ator, próxima ação +7 dias, canal derivado do UTM, `entrada_origem=formulario_entrada`).
- **Captura técnica**: honeypot (campo `website`), tempo mínimo 20s, rate limit por IP/hora (config `limite_entrada_por_ip_hora`, padrão 3; IP via realIp → X-Forwarded-For → Cf-Connecting-Ip; sem IP = fail-open com log), dedup por e-mail (negócio aberto <90 dias → registro `vinculado` + evento na oportunidade, sem criar duplicado).
- **Auditoria**: todo envio e vínculo geram evento em `auditoria` com snapshot mínimo (sem conteúdo do relato).
- Campo `entrada_origem` adicionado a `negocios` (0149).

### Provas (evidencias/spec-3-006/)

- RED 5: sem consentimento 400; tempo <20s 400; CNPJ inválido 400; GET leads sem auth 401; vincular sem auth 401; re-vincular 400.
- GREEN: envio válido 200 (score 92, quente); lista por temperatura 200; vincular 200 (contato + oportunidade criados); rate limit 429 no 4º envio do mesmo IP; honeypot 200 silencioso sem registro.
- Fixes no caminho: leitura de IP no JSVM (`e.request.header.get`, não `getHeader`) — 2 iterações provadas por API.
- Limpeza: migrations 0150/0151/0152 — base final 0 leads/contatos/negócios de prova.


## [0.0.448] — 2026-09-13 — T3.06 CONCLUÍDA (teste humano delegado aprovado)

- 2026-09-13 · [Deni.Ai] · Task T3.06 concluída: Automações Se/Então — cron diário 08:05 BRT gera follow-up de proposta (3/7 dias) e alertas de saúde (sem próxima ação, parada) em log append-only idempotente (coleção `automacoes_execucoes` 0147, UNIQUE regra+negócio+dia, create/update/delete bloqueados) + execução manual admin + leitura agrupada + seção "Automações de hoje" no painel Operacional. Provas RED/GREEN por API (401/403/400; fixture gerou follow_up_proposta com detalhe correto; idempotente; proposta decidida não gera; create direto 403; regressão comercial confirmada). Teste humano delegado pela CEO e executado no browser real: painel com 4 cards corretos e estado vazio explícito — print artifacts/t306_teste_humano_painel.png. Pendências fechadas: evidencias/spec-3-005/ (CA-3-015 a 017) + AP-2026-09-13-0920. QA verde v0.0.443→0.0.448.
- Aprendizado: AP-2026-09-13-0920-jsvm-cron-scoping.md (função top-level não é visível em callback de cron; QA do Skip bloqueia).

## [0.0.447] — 2026-09-13 — T3.06 Automações Se/Então implementada (aguardando teste humano)

### Adicionado (T3.06 — Automações Se/Então, SPEC-3-005, doc Onda 3 §12)
- **Coleção `automacoes_execucoes`** (migration 0147): regra (follow_up_proposta/follow_up_sem_resposta/alerta_sem_proxima_acao/alerta_parada), negócio, responsável, detalhe json (snapshot mínimo), dia_referencia. Create/update/delete bloqueados (null rules) — somente o servidor escreve. UNIQUE (regra+negócio+dia) — idempotência por dia.
- **Hook `automacoes_se_entao.js`** — cron diário 08:05 BRT (após o cron de propostas vencidas) + execução manual admin-only `POST /backend/v1/automacoes/executar` + leitura `GET /backend/v1/automacoes/execucoes?dia=YYYY-MM-DD` (agrupado por regra com contagem e lista). Regras: follow-up de proposta emitida há ≥3 dias sem decisão; SLA ≥7 dias; alerta de oportunidade ativa sem próxima ação futura; alerta de parada acima do limite configurado (reusa `limite_oportunidade_parada_dias`). GARANTIA T2.25: nunca altera resultado comercial.
- **UI no painel Operacional** — seção "Automações de hoje" no topo: 4 cards (contagem do dia + lista clicável das oportunidades); estado vazio explícito "Nada disparou hoje".

### Corrigido no caminho
- Scoping JSVM: função top-level não é visível em callback (QA bloqueou) — lógica duplicada inline em cada callback (AP-0200 reforçada) (v0.0.443).
- GET por dia: date PB armazena com hora — filtro por intervalo do dia em vez de igualdade (v0.0.444).
- UI: destructuring do Promise.all sem a 6ª variável — automacoesData undefined (v0.0.446).

### Provas (CA-3-015 a CA-3-017)
- RED: 401 sem auth (executar/GET) · 400 dia inválido · 403 execução manual não-admin · 403 create direto na coleção.
- GREEN: fixture (proposta emitida há 5 dias) → follow_up_proposta com detalhe {dias_desde_envio:5, versao:1, valor:9000}; reexecução idempotente (1 registro total); proposta com decisão (aceita) não gera execução; oportunidades saudáveis geram zero; regressão comercial confirmada (valor/estágio/status idênticos antes/depois).
- UI verificada no browser real: seção com 4 cards e execução da fixture visível — print artifacts/t306_automacoes_painel.png.
- Limpeza (0148): fixtures removidas — base final 0 execuções, 3 negócios reais.
- QA verde v0.0.443→0.0.447.

## [0.0.442] — 2026-09-13 — T3.05 CONCLUÍDA (teste humano delegado aprovado)

- 2026-09-13 · [Deni.Ai] · Task T3.05 concluída: E-mail P1 — registro estruturado de interações e-mail na oportunidade (coleção `interacoes_email` append-only 0145 com campo assunto 3–300, endpoints POST/GET server-side, UI no menu Mais ⌄, bloco E-mail na consulta 360º). Provas RED/GREEN por API (401/404/400/403; 200 POST+GET; 360º com bloco; auditoria 2 eventos; campos comerciais intocados). Limpeza 0146. Teste humano delegado pela CEO e executado no browser real: interação registrada pela UI apareceu na lista e na Consulta 360º (2 interações, assunto e próxima ação visíveis) — prints artifacts/t305_email_modal.png e artifacts/t305_360_email.png. QA verde v0.0.440→0.0.442.
- Aprendizado: AP-2026-09-13-0850 reaplicado (eval click() para itens de menu).

## [0.0.441] — 2026-09-13 — T3.05 E-mail P1 implementada (aguardando teste humano)

### Adicionado (T3.05 — E-mail P1, SPEC-3-004, doc Onda 3 §14)
- **Coleção `interacoes_email`** (migration 0145): negócio (relation obrigatória), contato (relation opcional), direção (entrada/saida), assunto (3–300 chars), resumo (5–5000 chars), resultado (sem_resposta/resposta/reuniao_agendada/proposta_solicitada/negativo), responsável, próxima ação (descrição + data), trilha json. Delete bloqueado (deleteRule null) — append-only.
- **Hook `email_interacoes.js`** — 2 rotas: POST `/backend/v1/email/interacoes` (auth: valida negócio existe/não arquivado, assunto, resumo, resultado, direção; auditoria com snapshot mínimo; próxima ação futura atualiza a oportunidade — só contexto, nunca campos comerciais; guard T2.18 intacto) e GET `/backend/v1/email/interacoes?negocio=X` (lista -created com nomes expandidos).
- **UI `EmailNegocio`** — botão "E-mail" no menu Mais ⌄ da oportunidade: registro (direção, assunto, resumo, resultado, próxima ação) + lista das interações.
- **Consulta 360º** — novo bloco `email` (total, última interação com assunto, próxima ação via e-mail) no endpoint e na UI.

### Provas (CA-3-012 a CA-3-014)
- RED: 401 sem auth · 404 negócio inexistente · 400 assunto curto · 400 resumo curto · 400 resultado inválido · 400 direção inválida · 400 GET sem negócio · 403 delete.
- GREEN: POST 200 (com e sem próxima ação; `proxima_acao_atualizada: true` e oportunidade refletindo; campos comerciais intocados) · GET 200 lista ordenada · 360º com bloco email · 2 eventos de auditoria.
- Limpeza (0146): fixtures de prova removidas, próxima ação original da Felicidade restaurada; caso real registrado (1 interação) para o teste humano.
- QA verde v0.0.440→0.0.441.

## [0.0.440] — 2026-09-13 — T3.04 CONCLUÍDA (teste humano delegado aprovado)

- 2026-09-13 · [Deni.Ai] · Task T3.04 concluída: Timeline 360º — consolidação cronológica server-side de 9 fontes existentes (endpoint GET /backend/v1/negocios/{id}/timeline, somente leitura, sem nova coleção) + UI no menu Mais ⌄ com badges por tipo. Provas RED/GREEN por API (401/404; Felicidade 8 eventos de 5 tipos em ordem desc; negócio simples 40 eventos; contagens das fontes inalteradas — somente leitura). Teste humano delegado pela CEO e executado no browser real: modal abriu com 6 eventos visíveis em ordem correta (Diagnóstico v1, WhatsApps, Handoff, Etapa, Entrada com origem/campanha) — print artifacts/t304_timeline_aberto.png. QA verde v0.0.439→0.0.440.
- Aprendizado: AP-2026-09-13-0850-agent-browser-menu-click.md.

## [0.0.439] — 2026-09-13 — T3.04 Timeline 360º implementada (aguardando teste humano)

### Adicionado (T3.04 — Timeline 360º, SPEC-3-003, doc Onda 3 §13)
- **Hook `timeline_endpoint.js`** — `GET /backend/v1/negocios/{id}/timeline` (auth, somente leitura): consolida eventos de 9 fontes existentes em lista cronológica desc com `{tipo, data, titulo, detalhe, autor}`. Resumo truncado a 200 chars; limite 300 eventos com flag `truncado`; falha de fonte vira aviso `fontes_com_erro`; sem nova coleção.
- **UI `TimelineNegocio`** — botão "Timeline" no menu Mais ⌄: linha do tempo vertical com badges por tipo, data pt-BR, detalhe e autor.

### Provas (CA-3-009 a CA-3-011)
- RED: 401 sem auth · 404 negócio inexistente.
- GREEN: Felicidade — 8 eventos de 5 tipos em ordem desc; negócio simples — 40 eventos; contagens das fontes idênticas antes/depois de 3 chamadas.
- QA verde v0.0.439.

## [0.0.438] — 2026-09-13 — T3.03 CONCLUÍDA (teste humano aprovado)

- 2026-09-13 · [Deni.Ai] · Task T3.03 concluída: WhatsApp P1 — registro estruturado de interações WhatsApp na oportunidade (coleção `interacoes_whatsapp` append-only 0142, endpoints POST/GET server-side, UI no menu Mais ⌄, bloco WhatsApp na consulta 360º). Provas RED/GREEN por API revalidadas do zero na conclusão (401/404/400/403; 200 POST+GET; 360º com bloco; auditoria com 5 eventos). Caso real: Felicidade Collective. Fix no caminho: `e.request.url.query()` no JSVM (v0.0.436). Limpezas 0143/0144 — base final com 2 interações reais. Teste humano aprovado pela CEO: "teste realizado e todos passaram". QA verde v0.0.435→0.0.438.
- Aprendizado: AP-2026-09-13-0810-jsvm-request-url-query.md.
- fase.md restaurado fielmente após erro de edição (sobrescrita truncada) — verificado byte a byte contra backup pré-edição; único diff é a atualização pretendida (T3.02b/T3.03 concluídas).

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
- Limpeza (0143): fixtures de prova removidas, próxima ação original da Felicidade restaurada; caso real registrado (1 interação, reuniao_agendada) para o teste humano.
- QA verde v0.0.435→0.0.437.

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