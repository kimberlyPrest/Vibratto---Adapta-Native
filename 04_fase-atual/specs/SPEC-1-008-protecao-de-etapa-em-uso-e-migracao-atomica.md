# SPEC-1-008 — Proteção de etapa em uso e migração atômica

**Fase:** 1  
**Status:** planejada  
**Dono:** Agente executor da Fase 1  
**Degrau da solução:** construção mínima — menor solução que produz o resultado observável sem antecipar fases futuras.

## Resultado observável

Etapas com oportunidades não são removidas sem destino ativo; a migração ocorre atomicamente e falhas preservam o estado original.

## Limites e dependências

- **Inclui:** comportamentos descritos nos critérios desta SPEC, seus estados de erro, recuperação e evidência.
- **Fora de escopo:** Sem migração entre múltiplos pipelines.
- **Entradas e pré-condições:** SPEC-1-006, SPEC-1-007 e SPEC-1-010.
- **Saídas/artefatos:** funcionalidade demonstrável, fixtures, roteiro de prova e evidências identificáveis.
- **Dependências e responsáveis:** executor implementa e prova; consultora valida mudanças de regra; cliente valida configuração de negócio quando aplicável.
- **Risco e plano B:** As dependências acima devem estar concluídas; usar somente fixtures enquanto G4 estiver pendente.
- **Rollback/reversão:** mudança reversível por configuração, arquivamento ou migração versionada; falha não apaga histórico.

## Fluxo e regras

1. Verificar as pré-condições e preparar fixtures específicas.
2. Executar o caminho principal e persistir o resultado.
3. Exercitar limites e erros obrigatórios sem deixar estado parcial.
4. Registrar evidência ligada a cada critério.

| Cenário | Condição | Resultado esperado | Recuperação |
|---|---|---|---|
| Principal | entradas válidas e ator autorizado | resultado observável persiste | repetir consulta e conferir estado |
| Limite | borda prevista nos critérios | regra específica é aplicada | corrigir entrada ou usar caminho alternativo |
| Falha | destino inativo, falha antes/durante confirmação, concorrência e cancelamento | nenhuma alteração indevida ou vazamento | estado anterior preservado e erro registrável |

## Checklist de execução

- [ ] Pré-condições verificadas.
- [ ] Caminho principal implementado.
- [ ] Limites e erros obrigatórios exercitados.
- [ ] Rollback/reversão demonstrado quando aplicável.
- [ ] Evidência individual de cada critério registrada.
- [ ] Handoff para a SPEC dependente confirmado.

## Critérios de aceite

- [ ] **CA-1-11 etapa em uso exige destino**.
- [ ] **CA-1-11A destino precisa estar ativo**.
- [ ] **CA-1-11B migração completa move todas**.
- [ ] **CA-1-11C falha injetada deixa zero estado parcial**.
- [ ] **CA-1-11D operação gera auditoria.**.


## TDD da SPEC

| Etapa | Prova | Ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | comportamento ausente ou regra violada | executar o primeiro cenário dos critérios antes da entrega | ao menos uma asserção ligada ao critério falha | `evidencias/spec-1-008/red.md` |
| GREEN | menor comportamento completo | executar roteiro critério a critério com fixtures nomeadas | todos os critérios desta SPEC passam individualmente | `evidencias/spec-1-008/green.md` + capturas/saídas |
| REFACTOR/REGRESSÃO | bordas, erro e reversão | repetir: destino inativo, falha antes/durante confirmação, concorrência e cancelamento | sem regressão, vazamento, duplicidade ou estado parcial | `evidencias/spec-1-008/regressao.md` |

**Fixtures:** massa fictícia mínima e identificada, com um caso principal, um limite e um erro para cada critério.  
**Caminhos de erro obrigatórios:** destino inativo, falha antes/durante confirmação, concorrência e cancelamento.  
**Evidência exigida:** roteiro CA a CA, capturas/saídas, estado antes/depois e índice local da SPEC.

## Tasks vinculadas

| Leva | ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|---|
| 13 | T8.1 | Implementar e demonstrar o caminho principal — Proteção de etapa em uso e migração atômica | Executor de software/dados | SPEC-1-008 | Caminho principal e critérios funcionais da SPEC-1-008 passam | GREEN — critérios principais da SPEC | evidencias/spec-1-008/t8.1-green.md | T6.2, T7.2 e T10.2 concluídas | ☐ Planejada |
| 14 | T8.2 | Validar bordas, segurança, reversão e evidências — Proteção de etapa em uso e migração atômica | Verificador da entrega | SPEC-1-008 | Erros, limites, rollback e índice de evidências da SPEC-1-008 passam | REFACTOR/REGRESSÃO + checklist completo | evidencias/spec-1-008/t8.2-regressao.md | T8.1 concluída | ☐ Planejada |
## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|