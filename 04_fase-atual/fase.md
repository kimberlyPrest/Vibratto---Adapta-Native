# Fase 1 — Fundação e CRM configurável (decomposição detalhada)

**Status:** pronta para execução após publicação  
**Resultado:** Central Comercial utilizável, segura, configurável sem código, auditável e portável.

## Incluído

Acesso e perfis; gate de dados; contatos; empresas; oportunidades; resultados comerciais; editor de etapas; kanban; migração atômica; contadores; tempo por etapa; filas; auditoria; busca; filtros; exportação e aceite integrado.

## Fora desta fase

Integrações externas, qualificação avançada, proposta, SLA, dashboard executivo, follow-up automático, aquisição, IA, múltiplos funis, importação massiva e operação financeira.

## SPECs

- **SPEC-1-001:** Acesso, sessão e perfis operacionais.
- **SPEC-1-002:** Modo demonstração, credenciais de teste e gate G4.
- **SPEC-1-003:** Contatos e empresas.
- **SPEC-1-004:** Oportunidades e campos comerciais.
- **SPEC-1-005:** Ganho, perda e reabertura.
- **SPEC-1-006:** Seed e editor administrativo de etapas.
- **SPEC-1-007:** Kanban e movimentação acessível.
- **SPEC-1-008:** Proteção de etapa em uso e migração atômica.
- **SPEC-1-009:** Contadores, tempo por etapa e filas operacionais.
- **SPEC-1-010:** Trilha de auditoria append-only.
- **SPEC-1-011:** Busca, filtros e recuperação operacional.
- **SPEC-1-012:** Exportação segura e aceite integrado.

## Demonstração integrada

Admin acessa → confirma modo demo → configura etapas → cadastra contato/empresa/oportunidade → move no kanban → registra perda e reabre → confere contadores/tempo/filas → pesquisa/filtra → audita → exporta → operador prova bloqueios → índice referencia todas as evidências.

## Tasks

| Leva | ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|---|
| 1 | T1.1 | Implementar e demonstrar o caminho principal — Acesso, sessão e perfis operacionais | Executor de software/dados | SPEC-1-001 | Caminho principal e critérios funcionais da SPEC-1-001 passam | GREEN — critérios principais da SPEC | evidencias/spec-1-001/t1.1-green.md | Projeto acessível | ✅ Concluída — 2026-09-04 |
| 2 | T1.2 | Validar bordas, segurança, reversão e evidências — Acesso, sessão e perfis operacionais | Verificador da entrega | SPEC-1-001 | Erros, limites, rollback e índice de evidências da SPEC-1-001 passam | REFACTOR/REGRESSÃO + checklist completo | evidencias/spec-1-001/t1.2-regressao.md | T1.1 concluída | ✅ Concluída — 2026-09-05 |
| 3 | T2.1 | Implementar e demonstrar o caminho principal — Modo demonstração, credenciais de teste e gate G4 | Executor de software/dados | SPEC-1-002 | Caminho principal e critérios funcionais da SPEC-1-002 passam | GREEN — critérios principais da SPEC | evidencias/spec-1-002/t2.1-green.md | T1.2 concluída e ambientes identificados | ✅ Concluída — 2026-09-05 |
| 4 | T2.2 | Validar bordas, segurança, reversão e evidências — Modo demonstração, credenciais de teste e gate G4 | Verificador da entrega | SPEC-1-002 | Erros, limites, rollback e índice de evidências da SPEC-1-002 passam | REFACTOR/REGRESSÃO + checklist completo | evidencias/spec-1-002/t2.2-regressao.md | T2.1 concluída | ☐ Planejada |
| 5 | T3.1 | Implementar e demonstrar o caminho principal — Contatos e empresas | Executor de software/dados | SPEC-1-003 | Caminho principal e critérios funcionais da SPEC-1-003 passam | GREEN — critérios principais da SPEC | evidencias/spec-1-003/t3.1-green.md | T2.2 concluída | ☐ Planejada |
| 6 | T3.2 | Validar bordas, segurança, reversão e evidências | Verificador da entrega | SPEC-1-003 | Erros, limites, rollback e índice de evidências da SPEC-1-003 passam | REFACTOR/REGRESSÃO + checklist completo | evidencias/spec-1-003/t3.2-regressao.md | T3.1 concluída | ☐ Planejada |
| 7 | T4.1 | Implementar e demonstrar o caminho principal — Oportunidades e campos comerciais | Executor de software/dados | SPEC-1-004 | Caminho principal e critérios funcionais da SPEC-1-004 passam | GREEN — critérios principais da SPEC | evidencias/spec-1-004/t4.1-green.md | T3.2 e T6.2 concluídas | ☐ Planejada |
| 8 | T4.2 | Validar bordas, segurança, reversão e evidências | Verificador da entrega | SPEC-1-004 | Erros, limites, rollback e índice de evidências da SPEC-1-004 passam | REFACTOR/REGRESSÃO + checklist completo | evidencias/spec-1-004/t4.2-regressao.md | T4.1 concluída | ☐ Planejada |
| 11 | T5.1 | Implementar e demonstrar o caminho principal — Ganho, perda e reabertura | Executor de software/dados | SPEC-1-005 | Caminho principal e critérios funcionais da SPEC-1-005 passam | GREEN — critérios principais da SPEC | evidencias/spec-1-005/t5.1-green.md | T4.2, T6.2 e T10.2 concluídas | ☐ Planejada |
| 12 | T5.2 | Validar bordas, segurança, reversão e evidências | Verificador da entrega | SPEC-1-005 | Erros, limites, rollback e índice de evidências da SPEC-1-005 passam | REFACTOR/REGRESSÃO + checklist completo | evidencias/spec-1-005/t5.2-regressao.md | T5.1 concluída | ☐ Planejada |
| 5 | T6.1 | Implementar e demonstrar o caminho principal — Seed e editor administrativo de etapas | Executor de software/dados | SPEC-1-006 | Caminho principal e critérios funcionais da SPEC-1-006 passam | GREEN — critérios principais da SPEC | evidencias/spec-1-006/t6.1-green.md | T1.2 concluída | ☐ Planejada |
| 6 | T6.2 | Validar bordas, segurança, reversão e evidências | Verificador da entrega | SPEC-1-006 | Erros, limites, rollback e índice de evidências da SPEC-1-006 passam | REFACTOR/REGRESSÃO + checklist completo | evidencias/spec-1-006/t6.2-regressao.md | T6.1 concluída | ☐ Planejada |
| 9 | T7.1 | Implementar e demonstrar o caminho principal — Kanban e movimentação acessível | Executor de software/dados | SPEC-1-007 | Caminho principal e critérios funcionais da SPEC-1-007 passam | GREEN — critérios principais da SPEC | evidencias/spec-1-007/t7.1-green.md | T4.2 e T6.2 concluídas | ☐ Planejada |
| 10 | T7.2 | Validar bordas, segurança, reversão e evidências | Verificador da entrega | SPEC-1-007 | Erros, limites, rollback e índice de evidências | REFACTOR/REGRESSÃO + checklist completo | evidencias/spec-1-007/t7.2-regressao.md | T7.1 concluída | ☐ Planejada |
| 13 | T8.1 | Implementar e demonstrar o caminho principal — Proteção de etapa em uso e migração atômica | Executor de software/dados | SPEC-1-008 | Caminho principal e critérios funcionais da SPEC-1-008 passam | GREEN — critérios principais da SPEC | evidencias/spec-1-008/t8.1-green.md | T6.2, T7.2 e T10.2 concluídas | ☐ Planejada |
| 14 | T8.2 | Validar bordas, segurança, reversão e evidências | Verificador da entrega | SPEC-1-008 | Erros, limites, rollback e índice de evidências | REFACTOR/REGRESSÃO + checklist completo | evidencias/spec-1-008/t8.2-regressao.md | T8.1 concluída | ☐ Planejada |
| 11 | T9.1 | Implementar e demonstrar o caminho principal — Contadores, tempo por etapa e filas operacionais | Executor de software/dados | SPEC-1-009 | Caminho principal e critérios funcionais da SPEC-1-009 passam | GREEN — critérios principais da SPEC | evidencias/spec-1-009/t9.1-green.md | T4.2 e T7.2 concluídas | ☐ Planejada |
| 12 | T9.2 | Validar bordas, segurança, reversão e evidências | Verificador da entrega | SPEC-1-009 | Erros, limites, rollback e índice de evidências da SPEC-1-009 passam | REFACTOR/REGRESSÃO + checklist completo | evidencias/spec-1-009/t9.2-regressao.md | T9.1 concluída | ☐ Planejada |
| 7 | T10.1 | Implementar e demonstrar o caminho principal — Trilha de auditoria append-only | Executor de software/dados | SPEC-1-010 | Caminho principal e critérios funcionais da SPEC-1-010 passam | GREEN — critérios principais da SPEC | evidencias/spec-1-010/t10.1-green.md | T1.2 concluída | ☐ Planejada |
| 8 | T10.2 | Validar bordas, segurança, reversão e evidências | Verificador da entrega | SPEC-1-010 | Erros, limites, rollback e índice de evidências | REFACTOR/REGRESSÃO + checklist completo | evidencias/spec-1-010/t10.2-regressao.md | T10.1 concluída | ☐ Planejada |
| 13 | T11.1 | Implementar e demonstrar o caminho principal — Busca, filtros e recuperação operacional | Executor de software/dados | SPEC-1-011 | Caminho principal e critérios funcionais da SPEC-1-011 passam | GREEN — critérios principais da SPEC | evidencias/spec-1-011/t11.1-green.md | T3.2, T4.2 e T9.2 concluídas | ☐ Planejada |
| 14 | T11.2 | Validar bordas, segurança, reversão e evidências | Verificador da entrega | SPEC-1-011 | Erros, limites, rollback e índice de evidências | REFACTOR/REGRESSÃO + checklist completo | evidencias/spec-1-011/t11.2-regressao.md | T11.1 concluída | ☐ Planejada |
| 15 | T12.1 | Implementar e demonstrar o caminho principal — Exportação segura e aceite integrado | Executor de software/dados | SPEC-1-012 | Caminho principal e critérios funcionais da SPEC-1-012 passam | GREEN — critérios principais da SPEC | evidencias/spec-1-012/t12.1-green.md | T5.2, T8.2, T9.2, T10.2 e T11.2 concluídas | ☐ Planejada |
| 16 | T12.2 | Validar bordas, segurança, reversão e evidências | Verificador da entrega | SPEC-1-012 | Erros, limites, rollback e índice de evidências da SPEC-1-012 passam | REFACTOR/REGRESSÃO + checklist completo | evidencias/spec-1-012/t12.2-regressao.md | T12.1 concluída | ☐ Planejada |
## Ordem de liberação

Cada task possui uma SPEC de origem. Tasks `.2` validam a entrega `.1` da mesma SPEC. Uma leva só abre quando as pré-condições listadas estiverem satisfeitas; mudança de intenção volta à SPEC.
