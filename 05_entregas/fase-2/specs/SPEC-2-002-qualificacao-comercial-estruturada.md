# SPEC-2-002 — Qualificação comercial estruturada

**Fase:** 2  
**Status:** planejada  
**Dono:** Produto/CRM  
**Origem no escopo:** Escopo definitivo §4 (qualificação estruturada) e §9 (G1 — Funil)  
**Degrau da solução:** recurso nativo da plataforma — ampliar oportunidade e etapas existentes sem novo serviço

## Resultado observável

A equipe registra uma qualificação reproduzível, identifica lacunas e impede avanço silencioso de oportunidades incompletas.

## Limites e dependências

- **Inclui:** campos configuráveis; respostas; completude; justificativa de exceção; desqualificação estruturada.
- **Fora de escopo:** score por IA, enriquecimento externo e decisão automática de qualificação.
- **Entradas e pré-condições:** dicionário homologado de perguntas obrigatórias, opcionais e condições de aplicabilidade.
- **Saídas/artefatos:** qualificação vinculada à oportunidade, estado de completude e histórico auditável.
- **Dependências e responsáveis:** dono da SPEC implementa; consultora homologa regra; cliente executa prova humana.
- **Risco e plano B:** Sem dicionário homologado, ativar configuração default desabilitada e usar fixture para prova.
- **Rollback ou reversão:** desativar formulário novo preservando respostas e histórico; nunca apagar qualificação já registrada.

## Fluxo e regras

1. Validar pré-condições, permissões e fixture isolada.
2. Executar o caminho principal e persistir o resultado observável.
3. Exercitar vazio, limite, permissão negada, concorrência e reversão.
4. Integrar ao histórico/auditoria e demonstrar no fluxo 360º.
5. Registrar evidência por critério e obter aceite humano.

1. Avanço exige campos aplicáveis completos ou exceção justificada.
2. Desqualificação exige motivo estruturado e observação quando motivo for Outro.
3. Toda alteração guarda ator, data e valores anterior/novo.

| Cenário | Dado/condição | Resultado esperado | Caminho de erro/recuperação |
|---|---|---|---|
| Principal | entrada válida e ator autorizado | resultado persiste e pode ser consultado | repetir consulta e comparar auditoria |
| Limite | vazio, borda temporal, tamanho máximo ou concorrência | regra explícita sem estado parcial | corrigir entrada ou reexecutar idempotentemente |
| Falha | ator sem permissão, transição inválida ou dependência indisponível | operação negada e estado anterior preservado | mensagem segura, log e ação de retomada |

## Checklist de execução

- [ ] Pré-condições e regra humana homologadas ou feature mantida desativada.
- [ ] Migration/schema reversível e fixture isolada criados.
- [ ] Caminho principal implementado e demonstrado.
- [ ] RBAC, LGPD, bordas, concorrência e rollback exercitados.
- [ ] Auditoria e integração 360º verificadas.
- [ ] Evidência individual de cada CA anexada.
- [ ] Build, lint e regressão da Fase 1 verdes.
- [ ] Aceite humano registrado.

## Critérios de aceite

- [ ] **CA-2-006:** administrador configura perguntas, obrigatoriedade, ordem e aplicabilidade sem código.
- [ ] **CA-2-007:** operador salva qualificação válida e visualiza percentual e pendências de completude.
- [ ] **CA-2-008:** operador não avança com campo obrigatório vazio; administrador só libera por exceção com motivo, validade e auditoria.
- [ ] **CA-2-009:** desqualificação registra motivo estruturado, detalhe obrigatório para Outro e próxima ação quando aplicável.
- [ ] **CA-2-010:** alterações e exceções aparecem na auditoria com ator, data e snapshots, inclusive tentativa negada.

## TDD da SPEC

| Etapa | CA | Comando/ação verificável | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | CA-2-006 | criar teste `spec-2-002-ca-2-006` com fixture específica e executar API/UI descrita em CA-2-006 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-002/ca-2-006-red.md` |
| GREEN | CA-2-006 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-006 passa integralmente | `evidencias/spec-2-002/ca-2-006-green.md` |
| RED | CA-2-007 | criar teste `spec-2-002-ca-2-007` com fixture específica e executar API/UI descrita em CA-2-007 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-002/ca-2-007-red.md` |
| GREEN | CA-2-007 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-007 passa integralmente | `evidencias/spec-2-002/ca-2-007-green.md` |
| RED | CA-2-008 | criar teste `spec-2-002-ca-2-008` com fixture específica e executar API/UI descrita em CA-2-008 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-002/ca-2-008-red.md` |
| GREEN | CA-2-008 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-008 passa integralmente | `evidencias/spec-2-002/ca-2-008-green.md` |
| RED | CA-2-009 | criar teste `spec-2-002-ca-2-009` com fixture específica e executar API/UI descrita em CA-2-009 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-002/ca-2-009-red.md` |
| GREEN | CA-2-009 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-009 passa integralmente | `evidencias/spec-2-002/ca-2-009-green.md` |
| RED | CA-2-010 | criar teste `spec-2-002-ca-2-010` com fixture específica e executar API/UI descrita em CA-2-010 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-002/ca-2-010-red.md` |
| GREEN | CA-2-010 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-010 passa integralmente | `evidencias/spec-2-002/ca-2-010-green.md` |
| REFACTOR/REGRESSÃO | todos | reexecutar todos os testes GREEN da SPEC-2-002, build/lint e roteiro integrado da Fase 1 | zero regressão e estado final consistente | `evidencias/spec-2-002/regressao.md` |

**Dados/fixtures:** duas contas fictícias (admin/operador), três empresas, cinco contatos e oito oportunidades cobrindo ativo, ganho, perda, incompleto, vencido e exceção; nenhum dado pessoal real antes de G4.  
**Caminhos de erro obrigatórios:** vazio, inválido, duplicado, não autorizado, usuário inativo, concorrência, timeout/repetição e rollback.  
**Evidência exigida:** log do teste/QA, resposta de API quando aplicável, captura do fluxo, diff auditável e aceite humano por CA.

## Tasks vinculadas

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|
| T2.11 | Implementar e provar CA-2-006 — administrador configura perguntas, obrigatoriedade, ordem e aplicabilidade sem código | Engenharia de produto | SPEC-2-002 | CA-2-006 passa integralmente, com estado final válido | CA-2-006 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-002/ca-2-006.md | SPEC-2-000 aceita; regra configurada com fixture e ativação real reservada à consultora/cliente | ☐ Planejada |
| T2.12 | Implementar e provar CA-2-007 — operador salva qualificação válida e visualiza percentual e pendências de completude | Engenharia de produto | SPEC-2-002 | CA-2-007 passa integralmente, com estado final válido | CA-2-007 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-002/ca-2-007.md | T2.11 concluída | ☐ Planejada |
| T2.13 | Implementar e provar CA-2-008 — operador não avança com campo obrigatório vazio; administrador só libera por exceção com motivo, validade e auditoria | Engenharia de produto | SPEC-2-002 | CA-2-008 passa integralmente, com estado final válido | CA-2-008 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-002/ca-2-008.md | T2.12 concluída | ☐ Planejada |
| T2.14 | Implementar e provar CA-2-009 — desqualificação registra motivo estruturado, detalhe obrigatório para Outro e próxima ação quando aplicável | Engenharia de produto | SPEC-2-002 | CA-2-009 passa integralmente, com estado final válido | CA-2-009 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-002/ca-2-009.md | T2.13 concluída | ☐ Planejada |
| T2.15 | Implementar e provar CA-2-010 — alterações e exceções aparecem na auditoria com ator, data e snapshots, inclusive tentativa negada | Engenharia de produto | SPEC-2-002 | CA-2-010 passa integralmente, com estado final válido | CA-2-010 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-002/ca-2-010.md | T2.14 concluída | ☐ Planejada |

## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
