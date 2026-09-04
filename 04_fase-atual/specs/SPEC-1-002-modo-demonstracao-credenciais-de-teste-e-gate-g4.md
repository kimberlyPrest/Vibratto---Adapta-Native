# SPEC-1-002 — Modo demonstração, credenciais de teste e gate G4

**Fase:** 1  
**Status:** planejada  
**Dono:** Agente executor da Fase 1  
**Degrau da solução:** recurso nativo da plataforma + configuração mínima — menor solução que produz o resultado observável sem antecipar fases futuras.

## Resultado observável

O ambiente de demonstração fica identificado e segregado; dados reais só entram após check G4; contas de teste são removidas ou desativadas antes da produção.

## Limites e dependências

- **Inclui:** comportamentos descritos nos critérios desta SPEC, seus estados de erro, recuperação e evidência.
- **Fora de escopo:** Sem definir a política jurídica de LGPD; esta SPEC aplica o gate aprovado.
- **Entradas e pré-condições:** SPEC-1-001 e ambientes identificados.
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
| Falha | tentativa de liberar produção sem check, conta .test ativa e fixture na base real | nenhuma alteração indevida ou vazamento | estado anterior preservado e erro registrável |

## Checklist de execução

- [ ] Pré-condições verificadas.
- [ ] Caminho principal implementado.
- [ ] Limites e erros obrigatórios exercitados.
- [ ] Rollback/reversão demonstrado quando aplicável.
- [ ] Evidência individual de cada critério registrada.
- [ ] Handoff para a SPEC dependente confirmado.

## Critérios de aceite

- [ ] **CA-1-05 modo demonstração visível**.
- [ ] **CA-1-05A produção bloqueada sem G4**.
- [ ] **CA-1-05B consulta reproduzível retorna zero contas .test ativas**.
- [ ] **CA-1-05C remoção/desativação é auditada**.
- [ ] **CA-1-05D dados fictícios não coexistem com base real.**.


## TDD da SPEC

| Etapa | Prova | Ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | comportamento ausente ou regra violada | executar o primeiro cenário dos critérios antes da entrega | ao menos uma asserção ligada ao critério falha | `evidencias/spec-1-002/red.md` |
| GREEN | menor comportamento completo | executar roteiro critério a critério com fixtures nomeadas | todos os critérios desta SPEC passam individualmente | `evidencias/spec-1-002/green.md` + capturas/saídas |
| REFACTOR/REGRESSÃO | bordas, erro e reversão | repetir: tentativa de liberar produção sem check, conta .test ativa e fixture na base real | sem regressão, vazamento, duplicidade ou estado parcial | `evidencias/spec-1-002/regressao.md` |

**Fixtures:** massa fictícia mínima e identificada, com um caso principal, um limite e um erro para cada critério.  
**Caminhos de erro obrigatórios:** tentativa de liberar produção sem check, conta .test ativa e fixture na base real.  
**Evidência exigida:** roteiro CA a CA, capturas/saídas, estado antes/depois e índice local da SPEC.

## Tasks vinculadas

| Leva | ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|---|
| 3 | T2.1 | Implementar e demonstrar o caminho principal — Modo demonstração, credenciais de teste e gate G4 | Executor de software/dados | SPEC-1-002 | Caminho principal e critérios funcionais da SPEC-1-002 passam | GREEN — critérios principais da SPEC | evidencias/spec-1-002/t2.1-green.md | T1.2 concluída e ambientes identificados | ☐ Planejada |
| 4 | T2.2 | Validar bordas, segurança, reversão e evidências — Modo demonstração, credenciais de teste e gate G4 | Verificador da entrega | SPEC-1-002 | Erros, limites, rollback e índice de evidências da SPEC-1-002 passam | REFACTOR/REGRESSÃO + checklist completo | evidencias/spec-1-002/t2.2-regressao.md | T2.1 concluída | ☐ Planejada |
## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|