# SPEC-2-006 — Handoff mínimo de ganho e pendências

**Fase:** 2  
**Status:** planejada  
**Dono:** Produto/Operação  
**Origem no escopo:** Escopo definitivo §4 (handoff mínimo do ganho) e §9 (G2 — Operação)  
**Degrau da solução:** construção mínima — checklist transacional ligado ao ganho já implementado

## Resultado observável

Uma oportunidade ganha gera um handoff verificável, com pacote mínimo, responsável receptor, aceite ou pendência explícita.

## Limites e dependências

- **Inclui:** checklist configurável; pacote de handoff; receptor; aceite; devolução; pendências.
- **Fora de escopo:** execução do serviço de BPO, financeiro, contratos e integração com sistemas de entrega.
- **Entradas e pré-condições:** oportunidade ganha, proposta vigente quando aplicável e dados mínimos homologados.
- **Saídas/artefatos:** handoff criado, aceite/devolução, pendências e histórico.
- **Dependências e responsáveis:** dono da SPEC implementa; consultora homologa regra; cliente executa prova humana.
- **Risco e plano B:** Sem segundo operador, permitir receptor nominal e registrar risco de dependência, sem fingir segregação.
- **Rollback ou reversão:** cancelar handoff com motivo e reabrir pendências sem desfazer o ganho automaticamente.

## Fluxo e regras

1. Validar pré-condições, permissões e fixture isolada.
2. Executar o caminho principal e persistir o resultado observável.
3. Exercitar vazio, limite, permissão negada, concorrência e reversão.
4. Integrar ao histórico/auditoria e demonstrar no fluxo 360º.
5. Registrar evidência por critério e obter aceite humano.

1. Ganho não desaparece por falha de handoff; gera pendência explícita.
2. Aceite exige receptor autenticado e checklist completo ou exceção autorizada.
3. Devolução exige motivo e dono da correção.

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

- [ ] **CA-2-026:** ganho cria handoff idempotente com checklist, origem, responsável emissor e receptor.
- [ ] **CA-2-027:** item obrigatório ausente impede aceite e gera pendência com dono e prazo.
- [ ] **CA-2-028:** receptor aceita ou devolve; decisão registra ator, data, motivo e snapshots.
- [ ] **CA-2-029:** repetição simultânea do evento de ganho cria exatamente um handoff e não sobrescreve decisão existente.
- [ ] **CA-2-030:** visão da oportunidade mostra estado do handoff, pendências abertas e tempo até aceite.

## TDD da SPEC

| Etapa | CA | Comando/ação verificável | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | CA-2-026 | criar teste `spec-2-006-ca-2-026` com fixture específica e executar API/UI descrita em CA-2-026 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-006/ca-2-026-red.md` |
| GREEN | CA-2-026 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-026 passa integralmente | `evidencias/spec-2-006/ca-2-026-green.md` |
| RED | CA-2-027 | criar teste `spec-2-006-ca-2-027` com fixture específica e executar API/UI descrita em CA-2-027 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-006/ca-2-027-red.md` |
| GREEN | CA-2-027 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-027 passa integralmente | `evidencias/spec-2-006/ca-2-027-green.md` |
| RED | CA-2-028 | criar teste `spec-2-006-ca-2-028` com fixture específica e executar API/UI descrita em CA-2-028 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-006/ca-2-028-red.md` |
| GREEN | CA-2-028 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-028 passa integralmente | `evidencias/spec-2-006/ca-2-028-green.md` |
| RED | CA-2-029 | criar teste `spec-2-006-ca-2-029` com fixture específica e executar API/UI descrita em CA-2-029 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-006/ca-2-029-red.md` |
| GREEN | CA-2-029 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-029 passa integralmente | `evidencias/spec-2-006/ca-2-029-green.md` |
| RED | CA-2-030 | criar teste `spec-2-006-ca-2-030` com fixture específica e executar API/UI descrita em CA-2-030 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-006/ca-2-030-red.md` |
| GREEN | CA-2-030 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-030 passa integralmente | `evidencias/spec-2-006/ca-2-030-green.md` |
| REFACTOR/REGRESSÃO | todos | reexecutar todos os testes GREEN da SPEC-2-006, build/lint e roteiro integrado da Fase 1 | zero regressão e estado final consistente | `evidencias/spec-2-006/regressao.md` |

**Dados/fixtures:** duas contas fictícias (admin/operador), três empresas, cinco contatos e oito oportunidades cobrindo ativo, ganho, perda, incompleto, vencido e exceção; nenhum dado pessoal real antes de G4.  
**Caminhos de erro obrigatórios:** vazio, inválido, duplicado, não autorizado, usuário inativo, concorrência, timeout/repetição e rollback.  
**Evidência exigida:** log do teste/QA, resposta de API quando aplicável, captura do fluxo, diff auditável e aceite humano por CA.

## Tasks vinculadas

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|
| T2.31 | Implementar e provar CA-2-026 — ganho cria handoff idempotente com checklist, origem, responsável emissor e receptor | Engenharia de produto | SPEC-2-006 | CA-2-026 passa integralmente, com estado final válido | CA-2-026 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-006/ca-2-026.md | SPEC-2-000 aceita; regra configurada com fixture e ativação real reservada à consultora/cliente | ☐ Planejada |
| T2.32 | Implementar e provar CA-2-027 — item obrigatório ausente impede aceite e gera pendência com dono e prazo | Engenharia de produto | SPEC-2-006 | CA-2-027 passa integralmente, com estado final válido | CA-2-027 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-006/ca-2-027.md | T2.31 concluída | ☐ Planejada |
| T2.33 | Implementar e provar CA-2-028 — receptor aceita ou devolve; decisão registra ator, data, motivo e snapshots | Engenharia de produto | SPEC-2-006 | CA-2-028 passa integralmente, com estado final válido | CA-2-028 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-006/ca-2-028.md | T2.32 concluída | ☐ Planejada |
| T2.34 | Implementar e provar CA-2-029 — repetição simultânea do evento de ganho cria exatamente um handoff e não sobrescreve decisão existente | Engenharia de produto | SPEC-2-006 | CA-2-029 passa integralmente, com estado final válido | CA-2-029 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-006/ca-2-029.md | T2.33 concluída | ☐ Planejada |
| T2.35 | Implementar e provar CA-2-030 — visão da oportunidade mostra estado do handoff, pendências abertas e tempo até aceite | Engenharia de produto | SPEC-2-006 | CA-2-030 passa integralmente, com estado final válido | CA-2-030 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-006/ca-2-030.md | T2.34 concluída | ☐ Planejada |

## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
