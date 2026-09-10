# SPEC-1-010 — Trilha de auditoria append-only

**Fase:** 1  
**Status:** planejada  
**Dono:** Agente executor da Fase 1  
**Degrau da solução:** recurso nativo + construção mínima — menor solução que produz o resultado observável sem antecipar fases futuras.

## Resultado observável

Ações críticas de autenticação, autorização, dados, funil e exportação geram eventos ordenados e imutáveis pela interface.

## Limites e dependências

- **Inclui:** comportamentos descritos nos critérios desta SPEC, seus estados de erro, recuperação e evidência.
- **Fora de escopo:** Sem logs de infraestrutura, SIEM ou backup corporativo.
- **Entradas e pré-condições:** SPEC-1-001 e eventos de domínio das demais SPECs.
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
| Falha | timestamps iguais, ator ausente, tentativa de edição/exclusão e presença de segredo | nenhuma alteração indevida ou vazamento | estado anterior preservado e erro registrável |

## Checklist de execução

- [ ] Pré-condições verificadas.
- [ ] Caminho principal implementado.
- [ ] Limites e erros obrigatórios exercitados.
- [ ] Rollback/reversão demonstrado quando aplicável.
- [ ] Evidência individual de cada critério registrada.
- [ ] Handoff para a SPEC dependente confirmado.

## Critérios de aceite

- [ ] **CA-1-13 evento tem ID, ator, timestamp, tipo e resultado**.
- [ ] **CA-1-13A antes/depois quando necessário**.
- [ ] **CA-1-13B ordem timestamp+ID**.
- [ ] **CA-1-13C evento não é editável/apagável**.
- [ ] **CA-1-13D senha/token nunca são registrados.**.


## TDD da SPEC

| Etapa | Prova | Ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | comportamento ausente ou regra violada | executar o primeiro cenário dos critérios antes da entrega | ao menos uma asserção ligada ao critério falha | `evidencias/spec-1-010/red.md` |
| GREEN | menor comportamento completo | executar roteiro critério a critério com fixtures nomeadas | todos os critérios desta SPEC passam individualmente | `evidencias/spec-1-010/green.md` + capturas/saídas |
| REFACTOR/REGRESSÃO | bordas, erro e reversão | repetir: timestamps iguais, ator ausente, tentativa de edição/exclusão e presença de segredo | sem regressão, vazamento, duplicidade ou estado parcial | `evidencias/spec-1-010/regressao.md` |

**Fixtures:** massa fictícia mínima e identificada, com um caso principal, um limite e um erro para cada critério.  
**Caminhos de erro obrigatórios:** timestamps iguais, ator ausente, tentativa de edição/exclusão e presença de segredo.  
**Evidência exigida:** roteiro CA a CA, capturas/saídas, estado antes/depois e índice local da SPEC.

## Tasks vinculadas

| Leva | ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|---|
| 7 | T10.1 | Implementar e demonstrar o caminho principal — Trilha de auditoria append-only | Executor de software/dados | SPEC-1-010 | Caminho principal e critérios funcionais da SPEC-1-010 passam | GREEN — critérios principais da SPEC | evidencias/spec-1-010/t10.1-green.md | T1.2 concluída | ✅ Concluída — 2026-09-08 |
| 8 | T10.2 | Validar bordas, segurança, reversão e evidências — Trilha de auditoria append-only | Verificador da entrega | SPEC-1-010 | Erros, limites, rollback e índice de evidências da SPEC-1-010 passam | REFACTOR/REGRESSÃO + checklist completo | evidencias/spec-1-010/t10.2-regressao.md | T10.1 concluída | ✅ Concluída — 2026-09-08 |
## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
