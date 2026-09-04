# SPEC-1-011 — Busca, filtros e recuperação operacional

**Fase:** 1  
**Status:** planejada  
**Dono:** Agente executor da Fase 1  
**Degrau da solução:** construção mínima — menor solução que produz o resultado observável sem antecipar fases futuras.

## Resultado observável

Usuários localizam oportunidades por texto e filtros combinados; registros arquivados são recuperáveis por perfil autorizado e não contaminam kanban/contadores.

## Limites e dependências

- **Inclui:** comportamentos descritos nos critérios desta SPEC, seus estados de erro, recuperação e evidência.
- **Fora de escopo:** Sem busca semântica, IA ou relatórios executivos.
- **Entradas e pré-condições:** SPEC-1-003, SPEC-1-004 e SPEC-1-009.
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
| Falha | busca vazia, combinação sem resultado, arquivado e usuário sem permissão | nenhuma alteração indevida ou vazamento | estado anterior preservado e erro registrável |

## Checklist de execução

- [ ] Pré-condições verificadas.
- [ ] Caminho principal implementado.
- [ ] Limites e erros obrigatórios exercitados.
- [ ] Rollback/reversão demonstrado quando aplicável.
- [ ] Evidência individual de cada critério registrada.
- [ ] Handoff para a SPEC dependente confirmado.

## Critérios de aceite

- [ ] **CA-1-14 busca textual retorna fixture**.
- [ ] **CA-1-14A filtros por etapa/origem/responsável/status**.
- [ ] **CA-1-14B combinação exata**.
- [ ] **CA-1-14C arquivados recuperáveis por filtro**.
- [ ] **CA-1-14D resultado vazio é explícito.**.


## TDD da SPEC

| Etapa | Prova | Ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | comportamento ausente ou regra violada | executar o primeiro cenário dos critérios antes da entrega | ao menos uma asserção ligada ao critério falha | `evidencias/spec-1-011/red.md` |
| GREEN | menor comportamento completo | executar roteiro critério a critério com fixtures nomeadas | todos os critérios desta SPEC passam individualmente | `evidencias/spec-1-011/green.md` + capturas/saídas |
| REFACTOR/REGRESSÃO | bordas, erro e reversão | repetir: busca vazia, combinação sem resultado, arquivado e usuário sem permissão | sem regressão, vazamento, duplicidade ou estado parcial | `evidencias/spec-1-011/regressao.md` |

**Fixtures:** massa fictícia mínima e identificada, com um caso principal, um limite e um erro para cada critério.  
**Caminhos de erro obrigatórios:** busca vazia, combinação sem resultado, arquivado e usuário sem permissão.  
**Evidência exigida:** roteiro CA a CA, capturas/saídas, estado antes/depois e índice local da SPEC.

## Tasks vinculadas

| Leva | ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|---|
| 13 | T11.1 | Implementar e demonstrar o caminho principal — Busca, filtros e recuperação operacional | Executor de software/dados | SPEC-1-011 | Caminho principal e critérios funcionais da SPEC-1-011 passam | GREEN — critérios principais da SPEC | evidencias/spec-1-011/t11.1-green.md | T3.2, T4.2 e T9.2 concluídas | ☐ Planejada |
| 14 | T11.2 | Validar bordas, segurança, reversão e evidências — Busca, filtros e recuperação operacional | Verificador da entrega | SPEC-1-011 | Erros, limites, rollback e índice de evidências da SPEC-1-011 passam | REFACTOR/REGRESSÃO + checklist completo | evidencias/spec-1-011/t11.2-regressao.md | T11.1 concluída | ☐ Planejada |
## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|