# SPEC-2-005 — Tarefas, SLAs e filas de exceção

**Fase:** 2  
**Status:** planejada  
**Dono:** Engenharia/Operação comercial  
**Origem no escopo:** Escopo definitivo §4 (tarefas, SLAs e filas) e §9 (G2 — Operação)  
**Degrau da solução:** reuso do que existe no repo — estender filas e contadores operacionais da Fase 1

## Resultado observável

O operador trabalha por tarefas e filas objetivas de atraso, ausência de próxima ação e exceções, com SLA configurável e sem disparo externo automático.

## Limites e dependências

- **Inclui:** tarefas; vencimento; prioridade; SLA por etapa/tipo; pausa justificada; filas.
- **Fora de escopo:** WhatsApp, e-mail, agenda externa e follow-up automático multicanal.
- **Entradas e pré-condições:** SLAs e calendário operacional homologados; usuários ativos; oportunidades existentes.
- **Saídas/artefatos:** tarefas vinculadas, relógios de SLA, filas e resolução auditada.
- **Dependências e responsáveis:** dono da SPEC implementa; consultora homologa regra; cliente executa prova humana.
- **Risco e plano B:** SLA sem calendário homologado gera falso atraso; iniciar com dias corridos explicitamente marcados.
- **Rollback ou reversão:** desativar regra de SLA preservando tarefas e eventos calculados.

## Fluxo e regras

1. Validar pré-condições, permissões e fixture isolada.
2. Executar o caminho principal e persistir o resultado observável.
3. Exercitar vazio, limite, permissão negada, concorrência e reversão.
4. Integrar ao histórico/auditoria e demonstrar no fluxo 360º.
5. Registrar evidência por critério e obter aceite humano.

1. SLA é configurável e versionado; mudança não reescreve histórico.
2. Pausa exige motivo, início, fim e ator.
3. Conclusão exige resultado e próxima ação quando a oportunidade continuar ativa.

| Cenário | Dado/condição | Resultado esperado | Caminho de erro/recuperação |
|---|---|---|---|
| Principal | entrada válida e ator autorizado | resultado persiste e pode ser consultado | repetir consulta e comparar auditoria |
| Limite | vazio, borda temporal, tamanho máximo ou concorrência | regra explícita sem estado parcial | corrigir entrada ou reexecutar idempotentemente |
| Falha | ator sem permissão, transição inválida ou dependência indisponível | operação negada e estado anterior preservado | mensagem segura, log e ação de retomada |

## Checklist de execução

- [ ] Pré-condições e regra humana homologadas ou feature mantida desativada.
- [ ] Migration/schema reversível e fixture isolada criados.
- [ ] Caminho principal implementado e demonstrado.
- [ ] RBAC, LGPD, bordas, concorrência e rollback exercitados.
- [ ] Auditoria e integração 360º verificadas.
- [ ] Evidência individual de cada CA anexada.
- [ ] Build, lint e regressão da Fase 1 verdes.
- [ ] Aceite humano registrado.

## Critérios de aceite

- [ ] **CA-2-021:** administrador configura SLA por evento/etapa com unidade, calendário e vigência, sem alterar histórico anterior.
- [ ] **CA-2-022:** operador cria, atribui, prioriza e conclui tarefa vinculada com resultado obrigatório.
- [ ] **CA-2-023:** tarefas vencidas, oportunidades sem próxima ação e exceções aparecem em filas distintas e reproduzíveis.
- [ ] **CA-2-024:** pausa, reabertura, usuário inativo e duas atualizações concorrentes preservam consistência e auditoria.
- [ ] **CA-2-025:** operador vê somente ações/registros permitidos; administrador consulta configuração e trilha completa.

## TDD da SPEC

| Etapa | CA | Comando/ação verificável | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | CA-2-021 | criar teste `spec-2-005-ca-2-021` com fixture específica e executar API/UI descrita em CA-2-021 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-005/ca-2-021-red.md` |
| GREEN | CA-2-021 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-021 passa integralmente | `evidencias/spec-2-005/ca-2-021-green.md` |
| RED | CA-2-022 | criar teste `spec-2-005-ca-2-022` com fixture específica e executar API/UI descrita em CA-2-022 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-005/ca-2-022-red.md` |
| GREEN | CA-2-022 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-022 passa integralmente | `evidencias/spec-2-005/ca-2-022-green.md` |
| RED | CA-2-023 | criar teste `spec-2-005-ca-2-023` com fixture específica e executar API/UI descrita em CA-2-023 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-005/ca-2-023-red.md` |
| GREEN | CA-2-023 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-023 passa integralmente | `evidencias/spec-2-005/ca-2-023-green.md` |
| RED | CA-2-024 | criar teste `spec-2-005-ca-2-024` com fixture específica e executar API/UI descrita em CA-2-024 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-005/ca-2-024-red.md` |
| GREEN | CA-2-024 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-024 passa integralmente | `evidencias/spec-2-005/ca-2-024-green.md` |
| RED | CA-2-025 | criar teste `spec-2-005-ca-2-025` com fixture específica e executar API/UI descrita em CA-2-025 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-005/ca-2-025-red.md` |
| GREEN | CA-2-025 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-025 passa integralmente | `evidencias/spec-2-005/ca-2-025-green.md` |
| REFACTOR/REGRESSÃO | todos | reexecutar todos os testes GREEN da SPEC-2-005, build/lint e roteiro integrado da Fase 1 | zero regressão e estado final consistente | `evidencias/spec-2-005/regressao.md` |

**Dados/fixtures:** duas contas fictícias (admin/operador), três empresas, cinco contatos e oito oportunidades cobrindo ativo, ganho, perda, incompleto, vencido e exceção; nenhum dado pessoal real antes de G4.  
**Caminhos de erro obrigatórios:** vazio, inválido, duplicado, não autorizado, usuário inativo, concorrência, timeout/repetição e rollback.  
**Evidência exigida:** log do teste/QA, resposta de API quando aplicável, captura do fluxo, diff auditável e aceite humano por CA.

## Tasks vinculadas

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|
| T2.26 | Implementar e provar CA-2-021 — administrador configura SLA por evento/etapa com unidade, calendário e vigência, sem alterar histórico anterior | Engenharia de produto | SPEC-2-005 | CA-2-021 passa integralmente, com estado final válido | CA-2-021 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-005/ca-2-021.md | SPEC-2-000 aceita; regra configurada com fixture e ativação real reservada à consultora/cliente | ☐ Planejada |
| T2.27 | Implementar e provar CA-2-022 — operador cria, atribui, prioriza e conclui tarefa vinculada com resultado obrigatório | Engenharia de produto | SPEC-2-005 | CA-2-022 passa integralmente, com estado final válido | CA-2-022 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-005/ca-2-022.md | T2.26 concluída | ☐ Planejada |
| T2.28 | Implementar e provar CA-2-023 — tarefas vencidas, oportunidades sem próxima ação e exceções aparecem em filas distintas e reproduzíveis | Engenharia de produto | SPEC-2-005 | CA-2-023 passa integralmente, com estado final válido | CA-2-023 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-005/ca-2-023.md | T2.27 concluída | ☐ Planejada |
| T2.29 | Implementar e provar CA-2-024 — pausa, reabertura, usuário inativo e duas atualizações concorrentes preservam consistência e auditoria | Engenharia de produto | SPEC-2-005 | CA-2-024 passa integralmente, com estado final válido | CA-2-024 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-005/ca-2-024.md | T2.28 concluída | ☐ Planejada |
| T2.30 | Implementar e provar CA-2-025 — operador vê somente ações/registros permitidos; administrador consulta configuração e trilha completa | Engenharia de produto | SPEC-2-005 | CA-2-025 passa integralmente, com estado final válido | CA-2-025 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-005/ca-2-025.md | T2.29 concluída | ☐ Planejada |

## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
