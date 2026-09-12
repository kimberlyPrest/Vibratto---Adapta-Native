# SPEC-3-004 — E-mail P1: registro estruturado de interações e-mail na oportunidade

**Task:** T3.05 (Fase 3 — leva 5)
**Base:** documento "Onda 3 — Conexão, Qualificação e Conversão" (§14 integrações prioritárias — e-mail), confirmado pela cliente como complementar em 12/09/2026.
**Princípio central (da cliente):** integração não é botão de acesso externo — o objetivo é centralizar contexto e histórico dentro do CRM.

## 1. Objetivo

Cada e-mail relevante ao negócio fica registrado no CRM como interação estruturada, vinculada à oportunidade e ao contato, com direção, assunto, resultado e próxima ação — mesmo padrão do WhatsApp P1 (T3.03), sem API externa nesta leva.

## 2. Escopo (recorte mínimo completo)

### 2.1 Coleção `interacoes_email` (nova)
- Campos: negocio (relation negocios, obrigatório), contato (relation clientes, opcional), direcao (select: entrada | saida), assunto (text, obrigatório, 3–300 chars), resumo (text, obrigatório, 5–5000 chars), resultado (select: sem_resposta | resposta | reuniao_agendada | proposta_solicitada | negativo), responsavel (relation users, obrigatório), proxima_acao_descricao (text, opcional, máx 1000), proxima_acao_em (date, opcional), trilha (json, append-only), created/updated.
- Regras de API: create/list/view/update apenas autenticado; **delete bloqueado** (deleteRule null) — append-only, mesmo padrão de interacoes_whatsapp.
- Índices: negocio, created.

### 2.2 Endpoint server-side
- `POST /backend/v1/email/interacoes` (auth): valida campos (negócio existe e não arquivado; assunto 3–300; resumo 5–5000; resultado válido; direção válida), grava interação, atualiza a oportunidade (proxima_acao_descricao/em quando futuras) e gera evento de auditoria com snapshot mínimo.
- `GET /backend/v1/email/interacoes?negocio={id}` (auth): lista as interações do negócio, mais recentes primeiro, com nomes expandidos.

### 2.3 UI na oportunidade
- Botão "E-mail" no menu Mais ⌄ do card da oportunidade (mesmo padrão de WhatsApp).
- Modal: lista das interações (data, direção, assunto, resultado, responsável) + formulário de registro (direção, assunto, resumo, resultado, próxima ação com data).
- Próxima ação futura atualiza a oportunidade (guard T2.18 preservado — save de sistema).

### 2.4 Integração com a consulta 360º
- A consulta 360º passa a incluir bloco `email`: total de interações, última interação (data/direção/assunto) e próxima ação registrada via e-mail.

### 2.5 Auditoria e LGPD
- Toda criação gera registro em `auditoria` (entidade, registro_id, ator, data, snapshot mínimo — nunca o conteúdo do resumo/assunto no log).

## 3. Fora do escopo desta task
- Sincronização automática via IMAP/SMTP ou API de provedor (leva futura — depende de decisão da cliente).
- Envio de e-mail pelo CRM (apenas registro).
- Instagram e agenda (levas seguintes).

## 4. Critérios de aceite e provas (TDD)

| ID | Critério | Prova |
|---|---|---|
| CA-3-012 | Interação e-mail registrada de forma estruturada, vinculada a negócio e contato | RED: POST sem auth 401; negócio inexistente 404; assunto curto 400; resumo curto 400; resultado inválido 400; direção inválida 400. GREEN: POST válido 200 → registro com vínculos; GET lista ordenada desc |
| CA-3-013 | Registro com próxima ação futura atualiza a oportunidade sem quebrar o guard T2.18 | RED: oportunidade sem próxima ação e-mail; GREEN: após registro, proxima_acao refletida; campos comerciais intocados (regressão) |
| CA-3-014 | Delete bloqueado e auditoria com ator/data | RED: DELETE 403; GREEN: evento de auditoria com ator e data em toda criação |

## 5. Riscos e cuidados
- Padrão provado na T3.03: lógica inline nos callbacks (AP-0200); query string via `e.request.url.query()` (AP-0810); save de sistema não dispara request hooks (guard T2.18 intacto).
- Datas PB normalizadas " " → "T"; 0001-01-01 = ausente.
- Fixtures de prova removidas ao final (padrão 0143/0144).

## 6. Evidência esperada
- evidencias/spec-3-004/ca-3-012.md … ca-3-014.md (RED/GREEN por API)
- Caso real: interação e-mail registrada para a oportunidade Felicidade Collective, visível na oportunidade e na consulta 360º.
- Teste humano: Deniane registra uma interação e-mail de teste e vê a lista e a próxima ação atualizada (ou delega, como na T3.04).
