# SPEC-1-003 — Contatos e empresas

**Fase:** 1  
**Status:** planejada  
**Dono:** Agente executor da Fase 1  
**Degrau da solução:** construção mínima — menor solução que produz o resultado observável sem antecipar fases futuras.

## Resultado observável

O operador cadastra, consulta, edita, arquiva e restaura contatos e empresas vinculados, com validação, origem/base legal quando aplicável e alerta não destrutivo de duplicidade.

## Limites e dependências

- **Inclui:** comportamentos descritos nos critérios desta SPEC, seus estados de erro, recuperação e evidência.
- **Fora de escopo:** Sem enriquecimento externo, consulta automática de CNPJ ou importação massiva.
- **Entradas e pré-condições:** SPEC-1-001 e modo demo da SPEC-1-002.
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
| Falha | e-mail/telefone inválido, duplicidade, texto malicioso, vínculo inexistente e restauração | nenhuma alteração indevida ou vazamento | estado anterior preservado e erro registrável |

## Checklist de execução

- [ ] Pré-condições verificadas.
- [ ] Caminho principal implementado.
- [ ] Limites e erros obrigatórios exercitados.
- [ ] Rollback/reversão demonstrado quando aplicável.
- [ ] Evidência individual de cada critério registrada.
- [ ] Handoff para a SPEC dependente confirmado.

## Critérios de aceite

- [ ] **CA-1-06 CRUD e vínculos**.
- [ ] **CA-1-06A formato e limites**.
- [ ] **CA-1-06B texto livre não executa**.
- [ ] **CA-1-06C origem/base legal**.
- [ ] **CA-1-06D duplicidade alerta sem fusão**.
- [ ] **CA-1-06E arquivo/restauração preserva histórico.**.


## TDD da SPEC

| Etapa | Prova | Ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | comportamento ausente ou regra violada | executar o primeiro cenário dos critérios antes da entrega | ao menos uma asserção ligada ao critério falha | `evidencias/spec-1-003/red.md` |
| GREEN | menor comportamento completo | executar roteiro critério a critério com fixtures nomeadas | todos os critérios desta SPEC passam individualmente | `evidencias/spec-1-003/green.md` + capturas/saídas |
| REFACTOR/REGRESSÃO | bordas, erro e reversão | repetir: e-mail/telefone inválido, duplicidade, texto malicioso, vínculo inexistente e restauração | sem regressão, vazamento, duplicidade ou estado parcial | `evidencias/spec-1-003/regressao.md` |

**Fixtures:** massa fictícia mínima e identificada, com um caso principal, um limite e um erro para cada critério.  
**Caminhos de erro obrigatórios:** e-mail/telefone inválido, duplicidade, texto malicioso, vínculo inexistente e restauração.  
**Evidência exigida:** roteiro CA a CA, capturas/saídas, estado antes/depois e índice local da SPEC.

## Tasks vinculadas

| Leva | ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|---|
| 5 | T3.1 | Implementar e demonstrar o caminho principal — Contatos e empresas | Executor de software/dados | SPEC-1-003 | Caminho principal e critérios funcionais da SPEC-1-003 passam | GREEN — critérios principais da SPEC | evidencias/spec-1-003/t3.1-green.md | T2.2 concluída | ☐ Planejada |
| 6 | T3.2 | Validar bordas, segurança, reversão e evidências — Contatos e empresas | Verificador da entrega | SPEC-1-003 | Erros, limites, rollback e índice de evidências da SPEC-1-003 passam | REFACTOR/REGRESSÃO + checklist completo | evidencias/spec-1-003/t3.2-regressao.md | T3.1 concluída | ☐ Planejada |
## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|