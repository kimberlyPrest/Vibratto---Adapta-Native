# SPEC-1-001 — Acesso, sessão e perfis operacionais

**Fase:** 1  
**Status:** planejada  
**Dono:** Agente executor da Fase 1  
**Degrau da solução:** recurso nativo da plataforma — menor solução que produz o resultado observável sem antecipar fases futuras.

## Resultado observável

Usuários autorizados acessam a Central Comercial com sessão individual; administradora e operador possuem permissões distintas e tentativas indevidas são bloqueadas.

## Limites e dependências

- **Inclui:** comportamentos descritos nos critérios desta SPEC, seus estados de erro, recuperação e evidência.
- **Fora de escopo:** Sem SSO, login social, MFA customizado ou recuperação por e-mail nesta fase.
- **Entradas e pré-condições:** projeto acessível e contas fictícias.
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
| Falha | credencial inválida, sessão ausente/expirada, operador em rota administrativa, conta desativada e ação forjada | nenhuma alteração indevida ou vazamento | estado anterior preservado e erro registrável |

## Checklist de execução

- [ ] Pré-condições verificadas.
- [ ] Caminho principal implementado.
- [ ] Limites e erros obrigatórios exercitados.
- [ ] Rollback/reversão demonstrado quando aplicável.
- [ ] Evidência individual de cada critério registrada.
- [ ] Handoff para a SPEC dependente confirmado.

## Critérios de aceite

- [ ] **CA-1-01 login válido/ inválido**.
- [ ] **CA-1-02 RBAC nas ações administrativas**.
- [ ] **CA-1-03 acesso administrativo**.
- [ ] **CA-1-04 logout e rota protegida**.
- [ ] **CA-1-04A sessão expirada e conta desativada**.
- [ ] **CA-1-04B requisição forjada não altera estado.**.


## TDD da SPEC

| Etapa | Prova | Ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | comportamento ausente ou regra violada | executar o primeiro cenário dos critérios antes da entrega | ao menos uma asserção ligada ao critério falha | `evidencias/spec-1-001/red.md` |
| GREEN | menor comportamento completo | executar roteiro critério a critério com fixtures nomeadas | todos os critérios desta SPEC passam individualmente | `evidencias/spec-1-001/green.md` + capturas/saídas |
| REFACTOR/REGRESSÃO | bordas, erro e reversão | repetir: credencial inválida, sessão ausente/expirada, operador em rota administrativa, conta desativada e ação forjada | sem regressão, vazamento, duplicidade ou estado parcial | `evidencias/spec-1-001/regressao.md` |

**Fixtures:** massa fictícia mínima e identificada, com um caso principal, um limite e um erro para cada critério.  
**Caminhos de erro obrigatórios:** credencial inválida, sessão ausente/expirada, operador em rota administrativa, conta desativada e ação forjada.  
**Evidência exigida:** roteiro CA a CA, capturas/saídas, estado antes/depois e índice local da SPEC.

## Tasks vinculadas

| Leva | ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|---|
| 1 | T1.1 | Implementar e demonstrar o caminho principal — Acesso, sessão e perfis operacionais | Executor de software/dados | SPEC-1-001 | Caminho principal e critérios funcionais da SPEC-1-001 passam | GREEN — critérios principais da SPEC | evidencias/spec-1-001/t1.1-green.md | Projeto acessível | ☐ Planejada |
| 2 | T1.2 | Validar bordas, segurança, reversão e evidências — Acesso, sessão e perfis operacionais | Verificador da entrega | SPEC-1-001 | Erros, limites, rollback e índice de evidências da SPEC-1-001 passam | REFACTOR/REGRESSÃO + checklist completo | evidencias/spec-1-001/t1.2-regressao.md | T1.1 concluída | ☐ Planejada |
## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|