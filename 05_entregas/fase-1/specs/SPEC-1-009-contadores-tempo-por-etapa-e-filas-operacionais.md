# SPEC-1-009 — Contadores, tempo por etapa e filas operacionais

**Fase:** 1  
**Status:** planejada  
**Dono:** Agente executor da Fase 1  
**Degrau da solução:** construção mínima — menor solução que produz o resultado observável sem antecipar fases futuras.

## Resultado observável

O sistema calcula contadores de oportunidades ativas, tempo acumulado por etapa e filas de próximas ações vencidas ou oportunidades paradas.

## Limites e dependências

- **Inclui:** comportamentos descritos nos critérios desta SPEC, seus estados de erro, recuperação e evidência.
- **Fora de escopo:** Sem BI, previsão, conversão, metas ou dashboard executivo da Fase 2.
- **Entradas e pré-condições:** SPEC-1-004 e SPEC-1-007.
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
| Falha | intervalo aberto, duas permanências, arquivada, data passada e limite de parado | nenhuma alteração indevida ou vazamento | estado anterior preservado e erro registrável |

## Checklist de execução

- [ ] Pré-condições verificadas.
- [ ] Caminho principal implementado.
- [ ] Limites e erros obrigatórios exercitados.
- [ ] Rollback/reversão demonstrado quando aplicável.
- [ ] Evidência individual de cada critério registrada.
- [ ] Handoff para a SPEC dependente confirmado.

## Critérios de aceite

- [ ] **CA-1-12 contador exclui arquivadas**.
- [ ] **CA-1-12A relógio controlado prova permanências**.
- [ ] **CA-1-12B tempo acumulado soma intervalos**.
- [ ] **CA-1-12C próxima ação passada entra em vencidas**.
- [ ] **CA-1-12D parado usa limite configurável e fixture exata.**.


## TDD da SPEC

| Etapa | Prova | Ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | comportamento ausente ou regra violada | executar o primeiro cenário dos critérios antes da entrega | ao menos uma asserção ligada ao critério falha | `evidencias/spec-1-009/red.md` |
| GREEN | menor comportamento completo | executar roteiro critério a critério com fixtures nomeadas | todos os critérios desta SPEC passam individualmente | `evidencias/spec-1-009/green.md` + capturas/saídas |
| REFACTOR/REGRESSÃO | bordas, erro e reversão | repetir: intervalo aberto, duas permanências, arquivada, data passada e limite de parado | sem regressão, vazamento, duplicidade ou estado parcial | `evidencias/spec-1-009/regressao.md` |

**Fixtures:** massa fictícia mínima e identificada, com um caso principal, um limite e um erro para cada critério.  
**Caminhos de erro obrigatórios:** intervalo aberto, duas permanências, arquivada, data passada e limite de parado.  
**Evidência exigida:** roteiro CA a CA, capturas/saídas, estado antes/depois e índice local da SPEC.

## Tasks vinculadas

| Leva | ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|---|
| 11 | T9.1 | Implementar e demonstrar o caminho principal — Contadores, tempo por etapa e filas operacionais | Executor de software/dados | SPEC-1-009 | Caminho principal e critérios funcionais da SPEC-1-009 passam | GREEN — critérios principais da SPEC | evidencias/spec-1-009/t9.1-green.md | T4.2 e T7.2 concluídas | ✅ Concluída — 2026-09-09 |
| 12 | T9.2 | Validar bordas, segurança, reversão e evidências — Contadores, tempo por etapa e filas operacionais | Verificador da entrega | SPEC-1-009 | Erros, limites, rollback e índice de evidências da SPEC-1-009 passam | REFACTOR/REGRESSÃO + checklist completo | evidencias/spec-1-009/t9.2-regressao.md | T9.1 concluída | ✅ Concluída — 2026-09-09 |
## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
