# Status

**Status:** Fase 2 em execução — 39 de 40 tasks concluídas (97,5%) — **SPEC-2-000 a SPEC-2-006 FECHADAS (41/41); SPEC-2-007 em execução (4/5)**
**Cliente:** Vibratto Assessoria Empresarial Ltda.
**Task ativa:** nenhuma
**Última task concluída:** T2.39 — CA-2-034, dado ausente aparece como cobertura incompleta e não é removido silenciosamente do denominador (2026-09-12, teste humano aprovado)
**Próxima task elegível:** T2.40 — CA-2-035 (SPEC-2-007 — drill-down e exportação agregada correspondem aos números exibidos, neutralizam fórmulas e respeitam RBAC/LGPD) — ÚLTIMA task da Fase 2
**Preview:** https://tela-de-login-crm-a400a--preview.goskip.app
**Produção:** não publicada

## Progresso da Fase 2 (resumo)

| Leva | Tasks | SPEC | Status |
| --- | --- | --- | --- |
| 1 | T2.01–T2.05 | SPEC-2-000 | ✅ Concluída — 2026-09-10 (fechada) |
| 2 | T2.06–T2.10 | SPEC-2-001 | ✅ Concluída — 2026-09-10 (fechada) |
| 3 | T2.11–T2.15 | SPEC-2-002 | ✅ Concluída — 2026-09-11 (fechada) |
| 4 | T2.16–T2.20 | SPEC-2-003 | ✅ Concluída — 2026-09-11 (fechada) |
| 5 | T2.21–T2.24 | SPEC-2-004 | ✅ Concluída — 2026-09-11 (fechada) |
| 6 | T2.25–T2.30 | SPEC-2-005 | ✅ Concluída — 2026-09-11 (fechada) |
| 7 | T2.31–T2.35 | SPEC-2-006 | ✅ Concluída — 2026-09-12 (fechada) |
| 8 | T2.36–T2.40 | SPEC-2-007 | 🔄 Em execução — 4/5 (T2.40 pendente) |

## SPEC-2-007 — métricas e dashboard (4/5)

| Task | Entrega | Status |
| --- | --- | --- |
| T2.36 | Dicionário de métricas (fórmula, fonte, eventos, fuso, exclusões, dono) | ✅ Concluída — 2026-09-12 (teste aprovado) |
| T2.37 | Baseline calculado para período explícito, congelado com versão e reproduzível | ✅ Concluída — 2026-09-12 (teste aprovado) |
| T2.38 | Dashboard comercial com N explícito e filtros consistentes | ✅ Concluída — 2026-09-12 (teste aprovado) |
| T2.39 | Dado ausente aparece como cobertura incompleta e não é removido do denominador | ✅ Concluída — 2026-09-12 (teste aprovado) |
| T2.40 | Drill-down e exportação agregada correspondem aos números exibidos, neutralizam fórmulas e respeitam RBAC/LGPD | ☐ Planejada |

## Evidência da T2.39 (concluída — teste humano aprovado em 2026-09-12)

- Bloco "Cobertura incompleta" no dashboard com avisos "permanecem no denominador" (3 de 3 / 2 de 3 / 1 de 3), badges COBERTURA X/Y por bloco, `sem_etapa: 1` explícito e conversão "50% (N=2)" com 3 oportunidades.
- Filtro Origem = Site recalcula todos os avisos sobre o denominador filtrado (2 de 2, conversão 0% N=1).
- RED: dado ausente não é criável por fora (proposta com valor 0 → 400, hook T2.21).
- Limpeza: migrations 0105/0106 removem fixtures e negócio órfão da prova.
- Evidência: `evidencias/spec-2-007/ca-2-034-green.md`.

## SPEC-2-002 — qualificação (COMPLETA, 5/5)

| Task  | Entrega                                                                                   | Status                                     |
| ----- | ----------------------------------------------------------------------------------------- | ------------------------------------------ |
| T2.11 | Configuração de perguntas de qualificação sem código (coleção + tela admin)               | ✅ Concluída — 2026-09-10 (teste aprovado) |
| T2.12 | Operador salva qualificação válida e visualiza percentual e pendências de completude      | ✅ Concluída — 2026-09-10 (teste aprovado) |
| T2.13 | Campo obrigatório bloqueia avanço; exceção de liberação com motivo, validade e auditoria  | ✅ Concluída — 2026-09-11 (teste aprovado) |
| T2.14 | Desqualificação com motivo estruturado e detalhe obrigatório para Outro                   | ✅ Concluída — 2026-09-11 (teste aprovado) |
| T2.15 | Alterações e exceções na auditoria com ator, data e snapshots, inclusive tentativa negada | ✅ Concluída — 2026-09-11 (teste aprovado) |

## Evidência da T2.12 (concluída — teste humano aprovado em 2026-09-10)

- Coleção `respostas_qualificacao` + endpoint server-side de completude + modal "Qualificar" na oportunidade.
- Fluxo ponta a ponta provado: 0% com pendência → resposta → 100% sem pendências.
- 5 defeitos de integração JSVM corrigidos e documentados (evidência RED).
- Evidências: `evidencias/spec-2-002/ca-2-007-red.md` / `ca-2-007-green.md`.

## Evidência da T2.11 (concluída — teste humano aprovado em 2026-09-10)

- Coleção `perguntas_qualificacao` (admin-only create/update, delete bloqueado — append-only).
- Tela admin `/admin/qualificacao` + links na home admin.
- Provas RED/GREEN por API (4 rejeições 400, create/update 200, RBAC operator bloqueado).
- Evidências: `evidencias/spec-2-002/ca-2-006-red.md` / `ca-2-006-green.md`.

## SPEC-2-001 — COMPLETA (5/5) — segurança de credenciais

| Task  | Entrega                                                                         |
| ----- | ------------------------------------------------------------------------------- |
| T2.06 | Senhas rotacionadas via secrets + busca automatizada de credenciais (achados 0) |
| T2.07 | Guard server-side de contas inativas (login/refresh bloqueados)                 |
| T2.08 | Build reproduzível: engines declaradas, typecheck, suíte vitest 22 testes       |
| T2.09 | Rotação reforçada idempotente (rejeita valores expostos)                        |
| T2.10 | Consulta reproduzível de aptidão para produção + limpeza do denominador real    |

## SPEC-2-000 — COMPLETA (5/5)

| Task  | Entrega                                                                                      |
| ----- | -------------------------------------------------------------------------------------------- |
| T2.01 | 8 campos comerciais em Oportunidades + validação server-side + auditoria de delete           |
| T2.02 | Empresa como entidade relacional (Opção A, aceite da consultora)                             |
| T2.03 | CSV neutralizado (=, +, -, @) + eventos append-only de exportação                            |
| T2.04 | Exportação server-side autorizada (aceite de uso único, quantidade recalculada, trilha)      |
| T2.05 | Auditoria com papel (operator só os próprios atos) + retenção 365d + cron + instalação limpa |

## Tasks anteriores (todas com evidências em evidencias/spec-2-000/ e spec-2-001/)

- T2.01–T2.05 (SPEC-2-000) e T2.06–T2.10 (SPEC-2-001) — concluídas em 2026-09-10.

## Fase 1

Arquivada em `05_entregas/fase-1/` com `phase-closure-manifest.json` (24/24 tasks).

## Limitações

Integrações externas, IA, dados reais e operação financeira permanecem fora do escopo até seus gates específicos. Produção liberada após SPEC-2-001 aceita (pre-production-check apto_producao=true); publicação aguarda decisão da cliente.
