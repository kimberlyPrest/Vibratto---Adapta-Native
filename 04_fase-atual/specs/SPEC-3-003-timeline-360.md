# SPEC-3-003 — Timeline 360º da relação (consolidação cronológica da oportunidade)

**Task:** T3.04 (Fase 3 — leva 4)
**Base:** documento "Onda 3 — Conexão, Qualificação e Conversão" (§13 Linha do tempo 360º), confirmado pela cliente como complementar em 12/09/2026.
**Princípio central (da cliente):** a visão 360º deve mostrar a história cronológica do relacionamento — entrada, origem, formulário, WhatsApp, e-mails, reuniões, diagnóstico, proposta, follow-ups, decisão e onboarding — sem montar isso de cabeça.

## 1. Objetivo

Uma única leitura cronológica de tudo que aconteceu com a oportunidade, consolidada server-side a partir das coleções que JÁ existem — sem nova coleção e sem duplicação de dados. O time vê a história do relacionamento em ordem, com tipo, data, autor e resumo de cada evento.

## 2. Escopo (recorte mínimo completo)

### 2.1 Endpoint server-side (somente leitura)
- `GET /backend/v1/negocios/{id}/timeline` (auth): consolida e ordena cronologicamente (mais recente primeiro) os eventos de:
  - `negocios`: entrada do lead (data_entrada/created), origem/campanha (canal → origem_especifica → campanha), mudanças de etapa (permanencias_negocio), pausa/reabertura (motivo_pausa/justificativa_reabertura quando preenchidos), decisão (data_ganho/motivo_ganho OU motivo_perda);
  - `formularios`: gerado, enviado, respondido (com solução);
  - `interacoes_whatsapp`: cada interação (direção, resultado, responsável);
  - `interacoes`: e-mail, reunião, ligação, outro (tipo, resumo, registrado_por);
  - `diagnosticos`: cada versão (versão, criado_por);
  - `propostas`: rascunho, emissão (versão, valor), decisão (aceita/recusada com ator);
  - `tarefas`: criação e conclusão (título, responsável);
  - `handoffs`: criação, aceite, devolução.
- Cada evento: `{ tipo, data, titulo, detalhe, autor }` — resumo truncado a 200 chars na listagem (conteúdo completo fica na tela própria de cada módulo).
- Resposta: `{ negocio_id, total, eventos: [...] }` ordenado por data desc; eventos sem data utilizável vão para o fim com data = created.
- 404 se negócio inexistente; 401 sem auth.

### 2.2 UI na oportunidade
- Botão "Timeline" no menu Mais ⌄ do card da oportunidade (mesmo padrão de Consulta 360º/WhatsApp).
- Modal cronológico: linha do tempo vertical com badge de tipo (Entrada, Etapa, Formulário, WhatsApp, E-mail, Reunião, Diagnóstico, Proposta, Tarefa, Handoff, Decisão), data formatada pt-BR, título, detalhe e autor.
- Estado vazio explícito: "Nenhum evento registrado além da entrada".

### 2.3 LGPD e performance
- Endpoint autenticado; nenhum dado novo é criado (somente leitura consolidada).
- Limite de 300 eventos por resposta (oportunidades longas não travam a tela); aviso explícito quando truncado.
- Nunca logar conteúdo de resumos no logger do servidor.

## 3. Fora do escopo desta task
- E-mail e Instagram como integrações (levas próprias — a timeline já exibe o que estiver registrado em `interacoes`).
- Pós-venda e LTV (leva própria).
- Filtros por tipo de evento (pode vir em refinamento futuro).
- Alteração em qualquer coleção existente — a timeline é somente leitura.

## 4. Critérios de aceite e provas (TDD)

| ID | Critério | Prova |
|---|---|---|
| CA-3-009 | Timeline consolida eventos de todas as fontes em ordem cronológica | RED: 401 sem auth; 404 negócio inexistente. GREEN: negócio com eventos em ≥3 fontes (ex.: Felicidade — entrada, WhatsApp, handoff, formulário) retorna lista ordenada desc com tipo/data/titulo/autor; total confere com a soma das fontes |
| CA-3-010 | Leitura explícita: nada é omitido ou maquiado | GREEN: negócio sem eventos além da entrada retorna 1 evento (entrada) — estado vazio explícito; evento sem data usa created; truncamento de 300 eventos sinalizado |
| CA-3-011 | Somente leitura — nenhuma coleção é alterada | RED: tentativa de write via timeline inexistente (rota única GET); regressão: contagens das coleções-fonte idênticas antes/depois da chamada |

## 5. Riscos e cuidados
- JSVM: query string via `e.request.url.query()` (AP-0810); lógica inline nos callbacks (AP-0200); finders em try/catch com fallback vazio — mas aqui falha de fonte NÃO pode sumir com eventos silenciosamente: fonte indisponível gera aviso no payload (`fontes_com_erro`).
- Datas PB: formato "2026-07-08 00:00:00.000Z" (com espaço) — normalizar com replace(' ','T') antes de Date.parse; 0001-01-01 = ausente.
- Performance: 8 fontes × findRecordsByFilter por requisição — limitar cada fonte a 100 registros e ordenar no servidor.
- Não alterar o endpoint consulta-360 existente (T2.20/T3.03) — timeline é endpoint novo.

## 6. Evidência esperada
- evidencias/spec-3-003/ca-3-009.md … ca-3-011.md (RED/GREEN por API)
- Caso real: timeline da Felicidade Collective exibindo entrada, WhatsApp (2 interações), handoff e decisão de ganho em ordem cronológica.
- Teste humano: Deniane abre a timeline da oportunidade e reconhece a história do relacionamento em ordem.
