# SPEC-1-005 — Ganho, perda e reabertura

**Fase:** 1  
**Status:** planejada  
**Dono:** Agente executor da Fase 1  
**Degrau da solução:** construção mínima — menor solução que produz o resultado observável sem antecipar fases futuras.

## Resultado observável

O resultado comercial é registrado de forma consistente: perda exige motivo estruturado; ganho registra o resultado; reabertura exige justificativa e destino ativo.

## Limites e dependências

- **Inclui:** comportamentos descritos nos critérios desta SPEC, seus estados de erro, recuperação e evidência.
- **Fora de escopo:** Sem decisão automática de ganho, perda, descarte ou negociação.
- **Entradas e pré-condições:** SPEC-1-004, SPEC-1-006 e auditoria da SPEC-1-010.
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
| Falha | perda sem motivo, destino inativo, reabertura sem justificativa e estado divergente | nenhuma alteração indevida ou vazamento | estado anterior preservado e erro registrável |

## Checklist de execução

- [ ] Pré-condições verificadas.
- [ ] Caminho principal implementado.
- [ ] Limites e erros obrigatórios exercitados.
- [ ] Rollback/reversão demonstrado quando aplicável.
- [ ] Evidência individual de cada critério registrada.
- [ ] Handoff para a SPEC dependente confirmado.

## Critérios de aceite

- [ ] **CA-1-08 perda exige motivo**.
- [ ] **CA-1-08A motivo aparece no histórico/exportação**.
- [ ] **CA-1-08B ganho não exige motivo de perda**.
- [ ] **CA-1-08C reabertura exige justificativa e etapa ativa**.
- [ ] **CA-1-08D reabertura preserva histórico.**.


## TDD da SPEC

| Etapa | Prova | Ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | comportamento ausente ou regra violada | executar o primeiro cenário dos critérios antes da entrega | ao menos uma asserção ligada ao critério falha | `evidencias/spec-1-005/red.md` |
| GREEN | menor comportamento completo | executar roteiro critério a critério com fixtures nomeadas | todos os critérios desta SPEC passam individualmente | `evidencias/spec-1-005/green.md` + capturas/saídas |
| REFACTOR/REGRESSÃO | bordas, erro e reversão | repetir: perda sem motivo, destino inativo, reabertura sem justificativa e estado divergente | sem regressão, vazamento, duplicidade ou estado parcial | `evidencias/spec-1-005/regressao.md` |

**Fixtures:** massa fictícia mínima e identificada, com um caso principal, um limite e um erro para cada critério.  
**Caminhos de erro obrigatórios:** perda sem motivo, destino inativo, reabertura sem justificativa e estado divergente.  
**Evidência exigida:** roteiro CA a CA, capturas/saídas, estado antes/depois e índice local da SPEC.

## Tasks vinculadas

| Leva | ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|---|
| 11 | T5.1 | Implementar e demonstrar o caminho principal — Ganho, perda e reabertura | Executor de software/dados | SPEC-1-005 | Caminho principal e critérios funcionais da SPEC-1-005 passam | GREEN — critérios principais da SPEC | evidencias/spec-1-005/t5.1-green.md | T4.2, T6.2 e T10.2 concluídas | ☐ Planejada |
| 12 | T5.2 | Validar bordas, segurança, reversão e evidências — Ganho, perda e reabertura | Verificador da entrega | SPEC-1-005 | Erros, limites, rollback e índice de evidências da SPEC-1-005 passam | REFACTOR/REGRESSÃO + checklist completo | evidencias/spec-1-005/t5.2-regressao.md | T5.1 concluída | ☐ Planejada |
## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|