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
| 5 | T2.05 | Implementar e provar CA-2-040 — auditoria registra ator, data, estado anterior/posterior com retenção 365 dias e leitura por papel | Engenharia/Segurança | SPEC-2-000 | CA-2-040 passa integralmente, com estado final válido | CA-2-040 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-000/ca-2-040.md | T2.04 concluída | ✅ Concluída — 2026-09-10 |
| 6 | T2.06 | Implementar e provar CA-2-041 — rotação de credenciais com escaneamento de repositório e zero exposição | Engenharia/Segurança | SPEC-2-000 | CA-2-041 passa integralmente, com estado final válido | CA-2-041 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-000/ca-2-041.md | T2.05 concluída | ✅ Concluída — 2026-09-10 (57 varridos, 0 achados; incidente de rota debug registrado e corrigido) |
| 7 | T2.07 | Implementar e provar CA-2-042 — guarda anti-enumeração de usuários (auth_active_guard) | Engenharia/Segurança | SPEC-2-000 | CA-2-042 passa integralmente, com estado final válido | CA-2-042 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-000/ca-2-042.md | T2.06 concluída | ✅ Concluída — 2026-09-10 |
| 8 | T2.08 | Implementar e provar CA-2-043 — suíte de 22 testes de segurança (injeção, IDOR, XSS, CSRF, rate limit, upload, LGPD) | Engenharia/Segurança | SPEC-2-000 | CA-2-043 passa integralmente, com estado final válido | CA-2-043 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-000/ca-2-043.md | T2.07 concluída | ✅ Concluída — 2026-09-10 |
| 9 | T2.09 | Implementar e provar CA-2-044 — rotação reforçada idempotente com verificação pós-rotação | Engenharia/Segurança | SPEC-2-000 | CA-2-044 passa integralmente, com estado final válido | CA-2-044 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-000/ca-2-044.md | T2.08 concluída | ✅ Concluída — 2026-09-10 |
| 10 | T2.10 | Implementar e provar CA-2-001 — qualificação estruturada com perguntas versionadas e exceções justificadas | Engenharia de produto | SPEC-2-001 | CA-2-001 passa integralmente, com estado final válido | CA-2-001 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-001/ca-2-001.md | Fase 2 liberada | ✅ Concluída — 2026-09-10 |
| 11 | T2.11 | Implementar e provar CA-2-002 — diagnóstico comercial versionado com campos estruturados | Engenharia de produto | SPEC-2-002 | CA-2-002 passa integralmente, com estado final válido | CA-2-002 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-002/ca-2-002.md | T2.10 concluída | ✅ Concluída — 2026-09-10 |
| 12 | T2.12 | Implementar e provar CA-2-003 — proposta com versões, decisão humana auditada e fila de vencidas | Engenharia de produto | SPEC-2-003 | CA-2-003 passa integralmente, com estado final válido | CA-2-003 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-003/ca-2-003.md | T2.11 concluída | ✅ Concluída — 2026-09-10 |
| 13 | T2.13 | Implementar e provar CA-2-004 — tarefas com SLA por evento e fila operacional | Engenharia de produto | SPEC-2-004 | CA-2-004 passa integralmente, com estado final válido | CA-2-004 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-004/ca-2-004.md | T2.12 concluída | ✅ Concluída — 2026-09-10 |
| 14 | T2.14 | Implementar e provar CA-2-005 — handoff mínimo de ganho com aceite do responsável | Engenharia de produto | SPEC-2-005 | CA-2-005 passa integralmente, com estado final válido | CA-2-005 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-005/ca-2-005.md | T2.13 concluída | ✅ Concluída — 2026-09-10 |
| 15 | T2.15 | Implementar e provar CA-2-006 — dicionário de métricas versionado com fórmulas do código real | Engenharia de produto | SPEC-2-006 | CA-2-006 passa integralmente, com estado final válido | CA-2-006 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-006/ca-2-006.md | T2.14 concluída | ✅ Concluída — 2026-09-10 |
| 16 | T2.16 | Implementar e provar CA-2-007 — baseline congelado com versão e período explícitos | Engenharia de produto | SPEC-2-006 | CA-2-007 passa integralmente, com estado final válido | CA-2-007 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-006/ca-2-007.md | T2.15 concluída | ✅ Concluída — 2026-09-10 |
| 17 | T2.17 | Implementar e provar CA-2-008 — dashboard comercial com 9 blocos, N e filtros consistentes | Engenharia de produto | SPEC-2-007 | CA-2-008 passa integralmente, com estado final válido | CA-2-008 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-007/ca-2-008.md | T2.16 concluída | ✅ Concluída — 2026-09-10 |
| 18 | T2.18 | Implementar e provar CA-2-009 — guarda comercial: próxima ação futura obrigatória em negócio ativo | Engenharia de produto | SPEC-2-007 | CA-2-009 passa integralmente, com estado final válido | CA-2-009 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-007/ca-2-009.md | T2.17 concluída | ✅ Concluída — 2026-09-10 |
| 19 | T2.19 | Implementar e provar CA-2-010 — cobertura de campos obrigatórios por bloco do dashboard | Engenharia de produto | SPEC-2-007 | CA-2-010 passa integralmente, com estado final válido | CA-2-010 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-007/ca-2-010.md | T2.18 concluída | ✅ Concluída — 2026-09-10 |
| 20 | T2.20 | Implementar e provar CA-2-011 — exceções de qualificação com motivo estruturado e auditoria | Engenharia de produto | SPEC-2-008 | CA-2-011 passa integralmente, com estado final válido | CA-2-011 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-008/ca-2-011.md | T2.19 concluída | ✅ Concluída — 2026-09-10 |
| 21 | T2.21 | Implementar e provar CA-2-012 — fila de propostas vencidas com alerta no painel | Engenharia de produto | SPEC-2-008 | CA-2-012 passa integralmente, com estado final válido | CA-2-012 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-008/ca-2-012.md | T2.20 concluída | ✅ Concluída — 2026-09-10 |
| 22 | T2.22 | Implementar e provar CA-2-013 — busca global e filtros combináveis | Engenharia de produto | SPEC-2-008 | CA-2-013 passa integralmente, com estado final válido | CA-2-013 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-008/ca-2-013.md | T2.21 concluída | ✅ Concluída — 2026-09-10 |
| 23 | T2.23 | Implementar e provar CA-2-014 — exportação com aceite de uso único e trilha completa | Engenharia/Segurança | SPEC-2-008 | CA-2-014 passa integralmente, com estado final válido | CA-2-014 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-008/ca-2-014.md | T2.22 concluída | ✅ Concluída — 2026-09-10 |
| 24 | T2.24 | Implementar e provar CA-2-015 — neutralização de fórmulas em CSV e eventos append-only | Engenharia/Segurança | SPEC-2-008 | CA-2-015 passa integralmente, com estado final válido | CA-2-015 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-008/ca-2-015.md | T2.23 concluída | ✅ Concluída — 2026-09-10 |
| 25 | T2.25 | Implementar e provar CA-2-016 — auditoria com leitura por papel e retenção verificável | Engenharia/Segurança | SPEC-2-008 | CA-2-016 passa integralmente, com estado final válido | CA-2-016 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-008/ca-2-016.md | T2.24 concluída | ✅ Concluída — 2026-09-10 |
| 26 | T2.26 | Implementar e provar CA-2-017 — SLA configurável por evento/etapa com fila vencida | Engenharia de produto | SPEC-2-009 | CA-2-017 passa integralmente, com estado final válido | CA-2-017 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-009/ca-2-017.md | T2.25 concluída | ✅ Concluída — 2026-09-10 |
| 27 | T2.27 | Implementar e provar CA-2-018 — filas operacionais distintas por tipo de pendência | Engenharia de produto | SPEC-2-009 | CA-2-018 passa integralmente, com estado final válido | CA-2-018 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-009/ca-2-018.md | T2.26 concluída | ✅ Concluída — 2026-09-10 |
| 28 | T2.28 | Implementar e provar CA-2-019 — painel operacional com contadores de filas e alertas | Engenharia de produto | SPEC-2-009 | CA-2-019 passa integralmente, com estado final válido | CA-2-019 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-009/ca-2-019.md | T2.27 concluída | ✅ Concluída — 2026-09-10 |
| 29 | T2.29 | Implementar e provar CA-2-020 — automação diária idempotente de follow-up e alertas de saúde | Engenharia de produto | SPEC-2-009 | CA-2-020 passa integralmente, com estado final válido | CA-2-020 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-009/ca-2-020.md | T2.28 concluída | ✅ Concluída — 2026-09-10 |
| 30 | T2.30 | Implementar e provar CA-2-021 — fila de oportunidades paradas com limite configurável | Engenharia de produto | SPEC-2-009 | CA-2-021 passa integralmente, com estado final válido | CA-2-021 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-009/ca-2-021.md | T2.29 concluída | ✅ Concluída — 2026-09-10 |
| 31 | T2.31 | Implementar e provar CA-2-022 — handoff com devolução justificada e reenvio | Engenharia de produto | SPEC-2-010 | CA-2-022 passa integralmente, com estado final válido | CA-2-022 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-010/ca-2-022.md | T2.30 concluída | ✅ Concluída — 2026-09-11 |
| 32 | T2.32 | Implementar e provar CA-2-023 — consulta 360º do negócio com blocos consolidados | Engenharia de produto | SPEC-2-010 | CA-2-023 passa integralmente, com estado final válido | CA-2-023 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-010/ca-2-023.md | T2.31 concluída | ✅ Concluída — 2026-09-11 |
| 33 | T2.33 | Implementar e provar CA-2-024 — reabertura de ganho com limpeza de motivo e estado válido | Engenharia de produto | SPEC-2-010 | CA-2-024 passa integralmente, com estado final válido | CA-2-024 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-010/ca-2-024.md | T2.32 concluída | ✅ Concluída — 2026-09-11 |
| 34 | T2.34 | Implementar e provar CA-2-025 — ciclo re-ganho por API com handoff único e snapshot preservado | Engenharia de produto | SPEC-2-010 | CA-2-025 passa integralmente, com estado final válido | CA-2-025 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-010/ca-2-025.md | T2.33 concluída | ✅ Concluída — 2026-09-11 |
| 35 | T2.35 | Implementar e provar CA-2-030 — bloco handoff na consulta 360º com estado e devoluções | Engenharia de produto | SPEC-2-006 | CA-2-030 passa integralmente, com estado final válido | CA-2-030 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-006/ca-2-030.md | T2.34 concluída | ✅ Concluída — 2026-09-11 |
| 36 | T2.36 | Implementar e provar CA-2-031 — dicionário de métricas com seed das fórmulas do código real e tela admin | Engenharia de produto | SPEC-2-007 | CA-2-031 passa integralmente, com estado final válido | CA-2-031 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-007/ca-2-031.md | T2.35 concluída | ✅ Concluída — 2026-09-11 |
| 37 | T2.37 | Implementar e provar CA-2-032 — baseline congelado com versão sequencial e período explícito | Engenharia de produto | SPEC-2-007 | CA-2-032 passa integralmente, com estado final válido | CA-2-032 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-007/ca-2-032.md | T2.36 concluída | ✅ Concluída — 2026-09-11 |
| 38 | T2.38 | Implementar e provar CA-2-033 — dashboard comercial com 9 blocos calculados sobre dados reais | Engenharia de produto | SPEC-2-007 | CA-2-033 passa integralmente, com estado final válido | CA-2-033 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-007/ca-2-033.md | T2.37 concluída | ✅ Concluída — 2026-09-11 |
| 39 | T2.39 | Implementar e provar CA-2-034 — cobertura incompleta reportada sem bloquear o dashboard | Engenharia de produto | SPEC-2-007 | CA-2-034 passa integralmente, com estado final válido | CA-2-034 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-007/ca-2-034.md | T2.38 concluída | ✅ Concluída — 2026-09-11 |
| 40 | T2.40 | Implementar e provar CA-2-035 — drill-down e exportação agregada do dashboard com modal e CSV | Engenharia de produto | SPEC-2-007 | CA-2-035 passa integralmente, com estado final válido | CA-2-035 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-007/ca-2-035.md | T2.39 concluída | ✅ Concluída — 2026-09-11 (teste humano aprovado 10:31) |

## Ordem de liberação

1. Leva 1 (T2.01–T2.09) — segurança e fundações;
2. Leva 2 (T2.10–T2.15) — processo comercial;
3. Leva 3 (T2.16–T2.25) — métricas e exportação;
4. Leva 4 (T2.26–T2.30) — operação e SLA;
5. Leva 5 (T2.31–T2.40) — consulta 360º e fechamento.

## Critério de aceite da fase — ATENDIDO

Caso real percorrido ponta a ponta; dashboard reproduz timestamps, cobertura e gargalos; auditoria, exportação e segurança provadas; baseline congelado; dicionário de métricas versionado.

# Fase 3 — Conexão, Qualificação e Conversão (em execução)

**Status:** EM EXECUÇÃO — 7 tasks concluídas ou em portão — aberta em 2026-09-12
**Base:** documento "Onda 3 — Conexão, Qualificação e Conversão" da cliente (confirmado COMPLEMENTAR às melhorias existentes — nada da Fase 1/2 é descartado).
**Resultado esperado:** o CRM deixa de apenas organizar a operação comercial e passa a capturar, centralizar e transformar interações em oportunidades e propostas.

## Tasks

| Leva | ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|---|
| 1 | T3.01 | Implementar e provar CA-3-001 — atribuição granular de origem (canal → origem específica → campanha → conteúdo) + motivo de ganho estruturado obrigatório no ganho; caso real Felicidade Collective como prova | Engenharia de produto | SPEC-3-000 | CA-3-001 passa integralmente, com estado final válido | RED/GREEN por API + caso real no dashboard | artifacts/T301_evidencia_ca3001.md | Fase 2 encerrada | ✅ Concluída — 2026-09-12 (teste humano aprovado 22:53 — "Agora, sim, TESTE REALIZADO") |
| 2 | T3.02 | Formulários inteligentes por solução (BPO/CFO/Consultoria) conectados ao CRM, com atualização automática da oportunidade | Engenharia de produto | SPEC-3-001 | CA-3-002 formulário público por solução com respostas vinculadas; CA-3-003 oportunidade atualizada automaticamente; CA-3-004 consentimento LGPD; CA-3-005 delete bloqueado e auditoria | RED/GREEN por API + caso real de formulário respondido | evidencias/spec-3-001/ + artifacts/T302_evidencia_ca3002.md | T3.01 concluída | ✅ Concluída — 2026-09-12 (teste humano aprovado 23:55 — "funcionou"; v0.0.418) |
| 2b | T3.02b | Ficha de preparação da proposta (consolidação interna antes da proposta) | Engenharia de produto | SPEC-3-001b | a definir na SPEC-3-001b | a definir | evidencias/spec-3-001b/ | T3.02 concluída | ✅ Concluída — 2026-09-13 00:16 (teste humano aprovado — "TASK VALIDADA"; v0.0.434) |
| 3 | T3.03 | WhatsApp P1 — registro estruturado de interações WhatsApp na oportunidade (coleção append-only, endpoint POST/GET, UI no menu Mais ⌄, bloco WhatsApp na consulta 360º) | Engenharia de produto | SPEC-3-002 | CA-3-006 interação estruturada vinculada a negócio/contato; CA-3-007 próxima ação atualiza a oportunidade sem quebrar guard T2.18; CA-3-008 delete bloqueado + auditoria | RED/GREEN por API + caso real na consulta 360º | evidencias/spec-3-002/ | T3.02b concluída | ✅ Concluída — 2026-09-13 08:10 (teste humano aprovado — "teste realizado e todos passaram"; v0.0.438) |
| 4 | T3.04 | Timeline 360º — consolidação cronológica server-side de 9 fontes existentes (endpoint somente leitura) + UI com badges por tipo | Engenharia de produto | SPEC-3-003 | CA-3-009 consolidação multi-fonte ordenada; CA-3-010 leitura explícita; CA-3-011 somente leitura | RED/GREEN por API + caso real na UI | evidencias/spec-3-003/ | T3.03 concluída | ✅ Concluída — 2026-09-13 08:55 (teste humano delegado aprovado com print; v0.0.440) |
| 5 | T3.05 | E-mail P1 — registro estruturado de interações e-mail (coleção append-only com assunto, endpoints POST/GET, UI no menu Mais ⌄, bloco E-mail na consulta 360º) | Engenharia de produto | SPEC-3-004 | CA-3-012 interação estruturada; CA-3-013 próxima ação sem quebrar guard T2.18; CA-3-014 delete bloqueado + auditoria | RED/GREEN por API + caso real na consulta 360º | evidencias/spec-3-004/ | T3.04 concluída | ✅ Concluída — 2026-09-13 09:05 (teste humano delegado aprovado com prints; v0.0.442) |
| 6 | T3.06 | Automações Se/Então — cron diário gera follow-up de proposta (3/7 dias) e alertas de saúde (sem próxima ação, parada) em log append-only idempotente + seção "Automações de hoje" no painel Operacional | Engenharia de produto | SPEC-3-005 | CA-3-015 execuções corretas e idempotentes; CA-3-016 alertas de saúde; CA-3-017 somente leitura comercial | GREEN por API + regressão comercial + UI no painel | evidencias/spec-3-005/ | T3.05 concluída | ⏳ Aguardando teste humano — implementada (v0.0.447, QA verde) |

## Correções autorizadas durante a T3.01 (registradas no changelog 0.0.380)

- Home: link "Abrir oportunidades" + cards Pipeline Comercial e Base de Contatos clicáveis (v0.0.374, v0.0.379).
- `servico` ganha Tesouraria — migration 0112/0113 + fix da causa raiz no hook `commercial_contract.js` (v0.0.375–0.0.378).

## Pós-conclusão da T3.02 (12/09)

- Formulários inteligentes implementados (v0.0.408–0.0.418): coleção `formularios`, 4 rotas por token, página pública `/formulario/:token`, UI no menu Mais ⌄, atualização automática da oportunidade (só contexto), LGPD obrigatório, auditoria.
- Debug autorizado no caminho: botão "Mais ⌄" não abria (listener global de clique) — corrigido em v0.0.415, verificado no browser real.
- Pendência técnica: delete de registros via migration ($app.delete) não removeu fixtures — resolvido por invalidação (status expirado); investigar causa raiz em debug futuro.

## Critério de aceite da Fase 3 — parcial (T3.01 ATENDIDO)

Atribuição capturada nos 4 níveis; motivo de ganho obrigatório e estruturado; dashboard alimenta leitura estratégica (de onde vêm e por que compram os clientes); caso real percorrido. Os demais critérios (formulários, integrações, timeline, pós-venda, dashboard executivo, IA) serão definidos SPEC a SPEC.

## Backlog complementar (aditivo — 12/09)

O documento "Requisitos complementares da etapa 3" da cliente (camada analítica, colaboração, V.ia, administração) foi registrado como backlog ADITIVO em `04_fase-atual/backlog-etapa3-requisitos-complementares.md`. Nada deste plano é substituído ou deletado; cada item entra como task formal com SPEC e autorização da CEO.
