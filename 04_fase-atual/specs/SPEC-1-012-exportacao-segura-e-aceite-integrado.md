# SPEC-1-012 — Exportação segura e aceite integrado

**Fase:** 1  
**Status:** planejada  
**Dono:** Agente executor da Fase 1  
**Degrau da solução:** recurso nativo + construção mínima — menor solução que produz o resultado observável sem antecipar fases futuras.

## Resultado observável

A administradora exporta dados essenciais em CSV/JSON com auditoria e proteção contra vazamento/fórmula; a fase fecha com demonstração única e índice das evidências.

## Limites e dependências

- **Inclui:** comportamentos descritos nos critérios desta SPEC, seus estados de erro, recuperação e evidência.
- **Fora de escopo:** Sem exportação contábil, BI, backup ou integrações externas.
- **Entradas e pré-condições:** SPEC-1-010, SPEC-1-011 e dados das SPECs anteriores.
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
| Falha | exportação negada, falha de geração, segredo, fórmula, contagem divergente e evidência ausente | nenhuma alteração indevida ou vazamento | estado anterior preservado e erro registrável |

## Checklist de execução

- [ ] Pré-condições verificadas.
- [ ] Caminho principal implementado.
- [ ] Limites e erros obrigatórios exercitados.
- [ ] Rollback/reversão demonstrado quando aplicável.
- [ ] Evidência individual de cada critério registrada.
- [ ] Handoff para a SPEC dependente confirmado.

## Critérios de aceite

- [ ] **CA-1-15 somente autorizado exporta**.
- [ ] **CA-1-15A negação é auditada**.
- [ ] **CA-1-15B evento registra formato/escopo/contagem/resultado**.
- [ ] **CA-1-16 IDs/contagens conferem**.
- [ ] **CA-1-16A CSV neutraliza = + - @**.
- [ ] **CA-1-16B nenhum segredo**.
- [ ] **CA-1-16C demo integrada e índice cobrem todas as SPECs sem repetir regressões.**.


## TDD da SPEC

| Etapa | Prova | Ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | comportamento ausente ou regra violada | executar o primeiro cenário dos critérios antes da entrega | ao menos uma asserção ligada ao critério falha | `evidencias/spec-1-012/red.md` |
| GREEN | menor comportamento completo | executar roteiro critério a critério com fixtures nomeadas | todos os critérios desta SPEC passam individualmente | `evidencias/spec-1-012/green.md` + capturas/saídas |
| REFACTOR/REGRESSÃO | bordas, erro e reversão | repetir: exportação negada, falha de geração, segredo, fórmula, contagem divergente e evidência ausente | sem regressão, vazamento, duplicidade ou estado parcial | `evidencias/spec-1-012/regressao.md` |

**Fixtures:** massa fictícia mínima e identificada, com um caso principal, um limite e um erro para cada critério.  
**Caminhos de erro obrigatórios:** exportação negada, falha de geração, segredo, fórmula, contagem divergente e evidência ausente.  
**Evidência exigida:** roteiro CA a CA, capturas/saídas, estado antes/depois e índice local da SPEC.

## Tasks vinculadas

| Leva | ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|---|
| 15 | T12.1 | Implementar e demonstrar o caminho principal — Exportação segura e aceite integrado | Executor de software/dados | SPEC-1-012 | Caminho principal e critérios funcionais da SPEC-1-012 passam | GREEN — critérios principais da SPEC | evidencias/spec-1-012/t12.1-green.md | T5.2, T8.2, T9.2, T10.2 e T11.2 concluídas | ☐ Planejada |
| 16 | T12.2 | Validar bordas, segurança, reversão e evidências — Exportação segura e aceite integrado | Verificador da entrega | SPEC-1-012 | Erros, limites, rollback e índice de evidências da SPEC-1-012 passam | REFACTOR/REGRESSÃO + checklist completo | evidencias/spec-1-012/t12.2-regressao.md | T12.1 concluída | ☐ Planejada |
## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|