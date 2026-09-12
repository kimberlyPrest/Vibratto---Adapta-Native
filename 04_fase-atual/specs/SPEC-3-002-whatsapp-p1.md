# SPEC-3-002 — WhatsApp P1: registro estruturado de interações WhatsApp na oportunidade

**Task:** T3.03 (Fase 3 — leva 3)
**Base:** documento "Onda 3 — Conexão, Qualificação e Conversão" (§12 automação, §13 timeline 360º, §14 integrações prioritárias), confirmado pela cliente como complementar em 12/09/2026.
**Princípio central (da cliente):** integração não é botão de acesso externo — o objetivo é centralizar contexto e histórico dentro do CRM.

## 1. Objetivo

Cada conversa de WhatsApp relevante ao negócio fica registrada no CRM como interação estruturada, vinculada à oportunidade e ao contato, com responsável, próxima ação e resultado — sem copiar e colar manualmente e sem depender de API externa nesta leva.

## 2. Escopo (recorte mínimo completo)

### 2.1 Coleção `interacoes_whatsapp` (nova)
- Campos: negocio (relation negocios, obrigatório), contato (relation clientes, opcional), direcao (select: entrada | saida), resumo (text, obrigatório, 5–5000 chars), resultado (select: sem_resposta | resposta | reuniao_agendada | proposta_solicitada | negativo), responsavel (relation users, obrigatório), proxima_acao_descricao (text, opcional), proxima_acao_em (date, opcional), trilha (json, append-only), created/updated.
- Regras de API: create/list/view/update apenas autenticado; **delete bloqueado** (deleteRule null) — append-only, coerente com o padrão do CRM (formularios, fichas_proposta).
- Índices: negocio, data de criação.

### 2.2 Endpoint server-side
- `POST /backend/v1/whatsapp/interacoes` (auth): valida campos (negócio existe e não arquivado; resumo ≥ 5 chars; resultado válido; responsável = ator autenticado), grava interação, atualiza a oportunidade (proxima_acao_descricao/proxima_acao_em quando informadas) e gera evento de auditoria com ator, data e snapshot mínimo.
- `GET /backend/v1/whatsapp/interacoes?negocio={id}` (auth): lista as interações do negócio, mais recentes primeiro.

### 2.3 UI na oportunidade
- Botão "WhatsApp" no menu Mais ⌄ do card da oportunidade (mesmo padrão de Formulário e Ficha da proposta).
- Modal: lista das interações do negócio (data, direção, resultado, responsável, resumo) + formulário de registro (direção, resumo, resultado, próxima ação com data).
- Ao registrar com próxima ação futura, a oportunidade reflete a nova próxima ação (guard T2.18 preservado — save de sistema não dispara request hooks).

### 2.4 Integração com a consulta 360º
- A consulta 360º (endpoint existente) passa a incluir bloco `whatsapp`: total de interações, última interação (data/direção/resultado) e próxima ação registrada via WhatsApp.

### 2.5 Auditoria e LGPD
- Toda criação gera registro em `auditoria` (entidade, registro_id, ator, data, snapshot mínimo — nunca o conteúdo integral do resumo no log).
- Resumo limitado a 5000 chars; conteúdo sensível não é logado no logger do servidor.

## 3. Fora do escopo desta task
- Sincronização automática via API oficial do WhatsApp Business / webhook (leva futura — depende de decisão da cliente sobre provedor).
- Envio de mensagens pelo CRM (apenas registro).
- Timeline 360º completa (leva própria, consumirá estas interações).
- E-mail, Instagram e agenda (levas seguintes).

## 4. Critérios de aceite e provas (TDD)

| ID | Critério | Prova |
|---|---|---|
| CA-3-006 | Interação WhatsApp registrada de forma estruturada, vinculada a negócio e contato, com responsável e resultado | RED: POST sem auth 401; negócio inexistente 404; resumo < 5 chars 400; resultado inválido 400. GREEN: POST válido 200 → registro com vínculos completos; GET lista ordenada |
| CA-3-007 | Registro com próxima ação futura atualiza a oportunidade sem quebrar o guard T2.18 | RED: oportunidade sem próxima ação WhatsApp; GREEN: após registro, proxima_acao_descricao/em refletidos; campos comerciais intocados (regressão) |
| CA-3-008 | Delete bloqueado e auditoria com ator/data | RED: DELETE 403; GREEN: evento de auditoria com ator e data em toda criação |

## 5. Riscos e cuidados
- Não quebrar o guard de negócio ativo (T2.18) ao atualizar a oportunidade via hook interno (save de sistema não dispara request hooks — lição T2.06).
- JSVM: callbacks não enxergam escopo superior — toda a lógica inline em cada callback (lição AP-2026-09-12-0200).
- JSVM: campo JSON chega como char codes — ler com JSON.parse(String(raw)) (lição T2.32).
- Migrations: atribuição direta `field.values = [...]` para selects (lição AP-0200).
- Mobile-first não se aplica à UI interna (uso pelo time no desktop); foco em clareza e velocidade de registro.

## 6. Evidência esperada
- evidencias/spec-3-002/ca-3-006.md … ca-3-008.md (RED/GREEN por API)
- Caso real: interação registrada para a oportunidade Felicidade Collective, visível na oportunidade e na consulta 360º.
- Teste humano: Deniane registra uma interação WhatsApp de teste na oportunidade e vê a lista e a próxima ação atualizada.
