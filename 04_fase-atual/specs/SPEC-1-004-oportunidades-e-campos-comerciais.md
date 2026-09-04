# SPEC-1-004 — Oportunidades e campos comerciais

**Fase:** 1  
**Status:** planejada  
**Dono:** Agente executor da Fase 1  
**Degrau da solução:** construção mínima — menor solução que produz o resultado observável sem antecipar fases futuras.

## Resultado observável

O operador cria e edita oportunidades vinculadas com origem, tags, responsável, etapa, prioridade, score, valor, serviço, próxima ação, status e data de entrada.

## Limites e dependências

- **Inclui:** comportamentos descritos nos critérios desta SPEC, seus estados de erro, recuperação e evidência.
- **Fora de escopo:** Sem qualificação avançada, proposta, SLA ou score por IA.
- **Entradas e pré-condições:** SPEC-1-003 e seed de etapas da SPEC-1-006.
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
| Falha | obrigatório vazio, score fora de 0–100, valor negativo, data inválida e status divergente | nenhuma alteração indevida ou vazamento | estado anterior preservado e erro registrável |

## Checklist de execução

- [ ] Pré-condições verificadas.
- [ ] Caminho principal implementado.
- [ ] Limites e erros obrigatórios exercitados.
- [ ] Rollback/reversão demonstrado quando aplicável.
- [ ] Evidência individual de cada critério registrada.
- [ ] Handoff para a SPEC dependente confirmado.

## Critérios de aceite

- [ ] **CA-1-07 CRUD e vínculos**.
- [ ] **CA-1-07A obrigatórios**.
- [ ] **CA-1-07B data de entrada automática**.
- [ ] **CA-1-07C opcionais e limites**.
- [ ] **CA-1-07D próxima ação inválida bloqueia e passada fica vencida**.
- [ ] **CA-1-07E status coerente com etapa.**.


## TDD da SPEC

| Etapa | Prova | Ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | comportamento ausente ou regra violada | executar o primeiro cenário dos critérios antes da entrega | ao menos uma asserção ligada ao critério falha | `evidencias/spec-1-004/red.md` |
| GREEN | menor comportamento completo | executar roteiro critério a critério com fixtures nomeadas | todos os critérios desta SPEC passam individualmente | `evidencias/spec-1-004/green.md` + capturas/saídas |
| REFACTOR/REGRESSÃO | bordas, erro e reversão | repetir: obrigatório vazio, score fora de 0–100, valor negativo, data inválida e status divergente | sem regressão, vazamento, duplicidade ou estado parcial | `evidencias/spec-1-004/regressao.md` |

**Fixtures:** massa fictícia mínima e identificada, com um caso principal, um limite e um erro para cada critério.  
**Caminhos de erro obrigatórios:** obrigatório vazio, score fora de 0–100, valor negativo, data inválida e status divergente.  
**Evidência exigida:** roteiro CA a CA, capturas/saídas, estado antes/depois e índice local da SPEC.

## Tasks vinculadas

| Leva | ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|---|
| 7 | T4.1 | Implementar e demonstrar o caminho principal — Oportunidades e campos comerciais | Executor de software/dados | SPEC-1-004 | Caminho principal e critérios funcionais da SPEC-1-004 passam | GREEN — critérios principais da SPEC | evidencias/spec-1-004/t4.1-green.md | T3.2 e T6.2 concluídas | ☐ Planejada |
| 8 | T4.2 | Validar bordas, segurança, reversão e evidências — Oportunidades e campos comerciais | Verificador da entrega | SPEC-1-004 | Erros, limites, rollback e índice de evidências da SPEC-1-004 passam | REFACTOR/REGRESSÃO + checklist completo | evidencias/spec-1-004/t4.2-regressao.md | T4.1 concluída | ☐ Planejada |
## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|