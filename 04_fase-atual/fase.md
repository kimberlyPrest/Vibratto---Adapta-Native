# Fase 2 — Processo comercial e baseline

**Status:** CONCLUÍDA — 40/40 tasks (100%), 8 SPECs fechadas — liberada em 2026-09-10, encerrada em 2026-09-12
**Resultado:** transformar o CRM em um processo operacional mensurável da qualificação ao handoff, sem automatizar julgamento comercial.

## Demonstração integrada

Caso real percorreu qualificação, diagnóstico, proposta, tarefas/SLA, ganho/perda e handoff; o dashboard reproduz timestamps, cobertura e gargalos.

## Incluído

- remediação verificável dos débitos canônicos da Fase 1;
- segurança de credenciais e build reproduzível;
- qualificação estruturada e exceções justificadas;
- diagnóstico comercial versionado;
- proposta com versões e decisão humana;
- tarefas, SLAs e filas operacionais;
- handoff mínimo de ganho;
- dashboard, baseline e dicionário de métricas.

## Fora desta fase

- integrações externas, WhatsApp, e-mail e agenda — Fase 3;
- aquisição, follow-up multicanal e lead magnet — Fase 4;
- IA, agente e autonomia — Fase 5;
- precificação, desconto ou negociação automáticos — fora do programa atual;
- operação financeira/BPO — fora do programa atual.

## Dependências homologadas para execução

As definições de qualificação, diagnóstico, SLA, handoff e métricas começam versionadas e configuráveis. Quando a regra humana ainda não estiver homologada, a feature permanece desativada e é provada com fixture; nenhuma regra é inventada pelo agente executor.

## Tasks

| Leva | ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|---|
| 1 | T2.01 | Implementar e provar CA-2-036 — oportunidades possuem origem, tags, responsável, prioridade, score, serviço, status e data de entrada, com validação e auditoria conforme contrato canônico F1 | Engenharia/Segurança | SPEC-2-000 | CA-2-036 passa integralmente, com estado final válido | CA-2-036 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-000/ca-2-036.md | Fase 1 encerrada e branch limpa | ✅ Concluída — 2026-09-10 |
| 2 | T2.02 | Implementar e provar CA-2-037 — empresa é entidade relacional própria ou decisão de manter campo textual está registrada com impacto, migração e aceite da consultora | Engenharia/Segurança | SPEC-2-000 | CA-2-037 passa integralmente, com estado final válido | CA-2-037 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-000/ca-2-037.md | T2.01 concluída | ✅ Concluída — 2026-09-10 (Opção A — entidade relacional, aceite da consultora) |
| 3 | T2.03 | Implementar e provar CA-2-038 — CSV neutraliza células iniciadas por =, +, - e @; cancelamento, negação e falha de exportação geram evento append-only | Engenharia/Segurança | SPEC-2-000 | CA-2-038 passa integralmente, com estado final válido | CA-2-038 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-000/ca-2-038.md | T2.02 concluída | ✅ Concluída — 2026-09-10 |
| 4 | T2.04 | Implementar e provar CA-2-039 — exportação de dados pessoais passa por endpoint server-side autorizado, com filtros/quantidade recalculados e trilha; acesso direto não contorna o aceite | Engenharia/Segurança | SPEC-2-000 | CA-2-039 passa integralmente, com estado final válido | CA-2-039 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-000/ca-2-039.md | T2.03 concluída | ✅ Concluída — 2026-09-10 |
| 5 | T2.05 | Implementar e provar CA-2-040 — instalação limpa aplica migrations sem IDs de ambiente; delete é auditado; leitura da auditoria respeita papel e retenção definida | Engenharia/Segurança | SPEC-2-000 | CA-2-040 passa integralmente, com estado final válido | CA-2-040 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-000/ca-2-040.md | T2.04 concluída | ✅ Concluída — 2026-09-10 (fecha SPEC-2-000) |
| 6 | T2.06 | Implementar e provar CA-2-001 — busca automatizada no snapshot e no histórico novo retorna zero senhas, tokens ou chaves fixas utilizáveis | Engenharia/Segurança | SPEC-2-001 | CA-2-001 passa integralmente, com estado final válido | CA-2-001 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-001/ca-2-001.md | SPEC-2-000 aceita; regra configurada com fixture e ativação real reservada à consultora/cliente | ✅ Concluída — 2026-09-10 |
| 7 | T2.07 | Implementar e provar CA-2-002 — login não contém ação ou valor que preencha senha; conta com active=false falha na autenticação server-side | Engenharia/Segurança | SPEC-2-001 | CA-2-002 passa integralmente, com estado final válido | CA-2-002 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-001/ca-2-002.md | T2.06 concluída | ✅ Concluída — 2026-09-10 |
| 8 | T2.08 | Implementar e provar CA-2-003 — exportação contém todos os imports locais; npm ci, typecheck, build, lint e suíte real terminam com código zero no Node declarado | Engenharia/Segurança | SPEC-2-001 | CA-2-003 passa integralmente, com estado final válido | CA-2-003 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-001/ca-2-003.md | T2.07 concluída | ✅ Concluída — 2026-09-10 |
| 9 | T2.09 | Implementar e provar CA-2-004 — ausência de secret obrigatório interrompe provisionamento sem criar conta parcial; rotação exige valor diferente do exposto | Engenharia/Segurança | SPEC-2-001 | CA-2-004 passa integralmente, com estado final válido | CA-2-004 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-001/ca-2-004.md | T2.08 concluída | ✅ Concluída — 2026-09-10 |
| 10 | T2.10 | Implementar e provar CA-2-005 — antes de produção, consulta reproduzível confirma zero contas/fixtures ativas e zero seeds de demonstração no denominador real | Engenharia/Segurança | SPEC-2-001 | CA-2-005 passa integralmente, com estado final válido | CA-2-005 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-001/ca-2-005.md | T2.09 concluída | ✅ Concluída — 2026-09-10 (fecha SPEC-2-001) |
| 11 | T2.11 | Implementar e provar CA-2-006 — administrador configura perguntas, obrigatoriedade, ordem e aplicabilidade sem código | Engenharia de produto | SPEC-2-002 | CA-2-006 passa integralmente, com estado final válido | CA-2-006 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-002/ca-2-006.md | SPEC-2-000 aceita; regra configurada com fixture e ativação real reservada à consultora/cliente | ✅ Concluída — 2026-09-11 |
| 12 | T2.12 | Implementar e provar CA-2-007 — operador salva qualificação válida e visualiza percentual e pendências de completude | Engenharia de produto | SPEC-2-002 | CA-2-007 passa integralmente, com estado final válido | CA-2-007 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-002/ca-2-007.md | T2.11 concluída | ✅ Concluída — 2026-09-11 |
| 13 | T2.13 | Implementar e provar CA-2-008 — operador não avança com campo obrigatório vazio; administrador só libera por exceção com motivo, validade e auditoria | Engenharia de produto | SPEC-2-002 | CA-2-008 passa integralmente, com estado final válido | CA-2-008 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-002/ca-2-008.md | T2.12 concluída | ✅ Concluída — 2026-09-11 |
| 14 | T2.14 | Implementar e provar CA-2-009 — desqualificação registra motivo estruturado, detalhe obrigatório para Outro e próxima ação quando aplicável | Engenharia de produto | SPEC-2-002 | CA-2-009 passa integralmente, com estado final válido | CA-2-009 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-002/ca-2-009.md | T2.13 concluída | ✅ Concluída — 2026-09-11 |
| 15 | T2.15 | Implementar e provar CA-2-010 — alterações e exceções aparecem na auditoria com ator, data e snapshots, inclusive tentativa negada | Engenharia de produto | SPEC-2-002 | CA-2-010 passa integralmente, com estado final válido | CA-2-010 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-002/ca-2-010.md | T2.14 concluída | ✅ Concluída — 2026-09-11 (fecha SPEC-2-002) |
| 16 | T2.16 | Implementar e provar CA-2-011 — operador cria versão de diagnóstico com núcleo mínimo e vínculo inequívoco à oportunidade | Engenharia de produto | SPEC-2-003 | CA-2-011 passa integralmente, com estado final válido | CA-2-011 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-003/ca-2-011.md | SPEC-2-000 aceita; regra configurada com fixture e ativação real reservada à consultora/cliente | ✅ Concluída — 2026-09-11 |
| 17 | T2.17 | Implementar e provar CA-2-012 — edição preserva versão anterior, ator, data e motivo da atualização | Engenharia de produto | SPEC-2-003 | CA-2-012 passa integralmente, com estado final válido | CA-2-012 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-003/ca-2-012.md | T2.16 concluída | ✅ Concluída — 2026-09-11 |
| 18 | T2.18 | Implementar e provar CA-2-013 — oportunidade ativa persiste responsável e próxima ação futura ou exceção vigente; a fila é responsabilidade da SPEC-2-005 | Engenharia de produto | SPEC-2-003 | CA-2-013 passa integralmente, com estado final válido | CA-2-013 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-003/ca-2-013.md | T2.17 concluída | ✅ Concluída — 2026-09-11 |
| 19 | T2.19 | Implementar e provar CA-2-014 — data passada, responsável inativo e texto acima de 5.000 caracteres são rejeitados sem estado parcial | Engenharia de produto | SPEC-2-003 | CA-2-014 passa integralmente, com estado final válido | CA-2-014 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-003/ca-2-014.md | T2.18 concluída | ✅ Concluída — 2026-09-11 |
| 20 | T2.20 | Implementar e provar CA-2-015 — consulta 360º exibe versão atual, histórico e campos ausentes explicitamente | Engenharia de produto | SPEC-2-003 | CA-2-015 passa integralmente, com estado final válido | CA-2-015 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-003/ca-2-015.md | T2.19 concluída | ✅ Concluída — 2026-09-11 (fecha SPEC-2-003) |
| 21 | T2.21 | Implementar e provar CA-2-016 — usuário autorizado cria rascunho com valor, validade, responsável e resumo válidos | Engenharia de produto | SPEC-2-004 | CA-2-016 passa integralmente, com estado final válido | CA-2-016 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-004/ca-2-016.md | SPEC-2-000 aceita; regra configurada com fixture e ativação real reservada à consultora/cliente | ✅ Concluída — 2026-09-11 |
| 22 | T2.22 | Implementar e provar CA-2-017 — emissão congel