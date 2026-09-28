# Status

**Status:** Fase 3 EM EXECUÇÃO — 13/N tasks concluídas
**Cliente:** Vibratto Assessoria Empresarial Ltda.
**Task ativa:** nenhuma
**Última task concluída:** T3.12 — motor de rotinas + exceções (teste humano executado)
**Próxima leva:** T3.13 — RBAC (SPEC-3-013 publicada) → T3.14 — V.ia estágio 1 (SPEC-3-014 publicada); depois: relatórios salvos/agendados, catálogo de serviços e backup com teste de restauração (backlog Etapa 3)
**Preview:** https://tela-de-login-crm-a400a--preview.goskip.app — formulário público em /entrada
**Versão atual:** v0.0.504 (QA verde)
**Produção:** não publicada (decisão da cliente)

## Composição da Fase 3 (em execução)

| Leva | Tasks | SPEC | Status |
| --- | --- | --- | --- |
| 1 | T3.01 + canais Comunidade/Spotify/Podcast | SPEC-3-000 | ✅ Concluída — 2026-09-12 |
| 2 | T3.02 — formulários inteligentes | SPEC-3-001 | ✅ Concluída — 2026-09-13 |
| 2b | T3.02b — ficha de preparação da proposta | SPEC-3-001b | ✅ Concluída — 2026-09-13 00:16 |
| 3 | T3.03 — WhatsApp P1 | SPEC-3-002 | ✅ Concluída — 2026-09-13 08:10 |
| 4 | T3.04 — Timeline 360º | SPEC-3-003 | ✅ Concluída — 2026-09-13 08:55 |
| 5 | T3.05 — E-mail P1 | SPEC-3-004 | ✅ Concluída — 2026-09-13 09:05 |
| 6 | T3.06 — Automações Se/Então (§12) | SPEC-3-005 | ✅ Concluída — 2026-09-13 09:25 |
| 7 | T3.07 — Porta 1, formulário de entrada | SPEC-3-006 | ✅ Concluída — 2026-09-13 10:06 |
| 8 | T3.08 — fila de trabalho pessoal + comentários/menções | SPEC-3-007 | ✅ Concluída — 2026-09-13 11:23 |
| 9 | T3.09 — harmonização visual dos cards | SPEC-3-008 | ✅ Concluída — 2026-09-13 11:38 |
| 10 | T3.10 — painel por papel + metas + comparativo | SPEC-3-010 | ✅ Concluída — 2026-09-13 12:17 |
| 11 | T3.11 — Ficha Operacional do Cliente (Leva A) | SPEC-3-011 | ✅ Concluída — teste humano executado |
| 12 | T3.12 — Motor de Rotinas + Exceções (Leva B) | SPEC-3-012 | ✅ Concluída — teste humano executado |
| 12b | T3.13 — RBAC: perfis, permissões e gestão de usuários (Rodada 1) | SPEC-3-013 | 📋 Publicada — aguardando execução |
| 13 | T3.14 — V.ia estágio 1: WhatsApp (API oficial) + Gemini (Rodada 2) | SPEC-3-014 | 📋 Publicada — aguardando execução |

## Governança (2026-09-28)

- Fase 2 (40/40, 8 SPECs) arquivada em `05_entregas/fase-2/` com phase-closure-manifest; `04_fase-atual/` contém somente a Fase 3.
- SPEC-3-013 e SPEC-3-014 publicadas conforme o documento da CEO de 23/09 e a direção da consultora (RBAC primeiro; integrações de canais da seção 9 em spec própria).
- `via_config` nasce inativo — ativação real reservada à consultora/cliente após segredos (GEMINI_API_KEY, WHATSAPP_TOKEN, WHATSAPP_APP_SECRET) e número comercial configurados.

## Decisões da CEO (13/09)

- **D6 — notificação de lead quente**: inicialmente somente a Deniane (CEO).
- **D8 — agenda na tela final do formulário**: Calendly; fica para depois (fora do recorte atual).
- **D2 — relato livre**: opcional, com mínimo de 30 caracteres se preenchido (decisão da CEO, 13/09 — rejeitado o mínimo de 120 chars por custo de conversão; revisável com dado real de uso). **IMPLEMENTADA E TESTADA** (v0.0.455–0.0.459; teste humano aprovado 10:47).
- **D5 — retenção de leads que não fecharam**: 24 meses da coleta ou do último contato, o que for mais recente; eliminação dos dados de identificação ao fim do prazo (decisão da CEO, 13/09). **IMPLEMENTADA E TESTADA** (v0.0.456–0.0.459; teste humano aprovado 10:47).

## Limitações

Dedup por e-mail provado por API e no teste humano. Instagram, agenda, pós-venda, dashboard executivo e IA nas próximas levás. Publicação em produção aguarda decisão da cliente.
