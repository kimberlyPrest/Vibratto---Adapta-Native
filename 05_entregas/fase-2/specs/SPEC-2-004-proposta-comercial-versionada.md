# SPEC-2-004 — Proposta comercial versionada e decisão humana

**Fase:** 2  
**Status:** planejada  
**Dono:** Produto/Comercial  
**Origem no escopo:** Escopo definitivo §4 (proposta versionada e decisão humana)  
**Degrau da solução:** construção mínima — modelar proposta e versões no PocketBase, reutilizando oportunidade e auditoria

## Resultado observável

A equipe controla propostas por versão, valor, validade, status e responsável, preservando decisão e negociação humanas.

## Limites e dependências

- **Inclui:** rascunho; versões; itens/resumo; valor; validade; status; aceite/recusa registrados.
- **Fora de escopo:** precificação automática, desconto automático, assinatura eletrônica e emissão financeira.
- **Entradas e pré-condições:** diagnóstico mínimo válido e oportunidade ativa.
- **Saídas/artefatos:** proposta versionada, status corrente, trilha e vínculo com oportunidade.
- **Dependências e responsáveis:** dono da SPEC implementa; consultora homologa regra; cliente executa prova humana.
- **Risco e plano B:** Referências externas não são armazenadas nesta fase; anexos permanecem fora do escopo.
- **Rollback ou reversão:** arquivar proposta e restaurar status anterior da oportunidade por ação auditada.

## Fluxo e regras

1. Validar pré-condições, permissões e fixture isolada.
2. Executar o caminho principal e persistir o resultado observável.
3. Exercitar vazio, limite, permissão negada, concorrência e reversão.
4. Integrar ao histórico/auditoria e demonstrar no fluxo 360º.
5. Registrar evidência por critério e obter aceite humano.

1. Versão emitida é imutável; alteração cria nova versão.
2. Aprovação, desconto e negociação são decisões humanas.
3. Validade vencida muda a fila, não altera resultado comercial automaticamente.

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

- [ ] **CA-2-016:** usuário autorizado cria rascunho com valor, validade, responsável e resumo válidos.
- [ ] **CA-2-017:** emissão congela a versão; mudança posterior cria número sequencial sem sobrescrever histórico.
- [ ] **CA-2-018:** status inválido, valor negativo, validade passada ou emissão concorrente são bloqueados atomicamente.
- [ ] **CA-2-019:** aceite ou recusa registra ator, data, canal, observação e mantém a decisão como humana.
- [ ] **CA-2-020:** todo dia às 08:00 no fuso America/Sao_Paulo, proposta vencida aparece na fila do responsável sem alterar resultado comercial.

## TDD da SPEC

| Etapa | CA | Comando/ação verificável | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | CA-2-016 | criar teste `spec-2-004-ca-2-016` com fixture específica e executar API/UI descrita em CA-2-016 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-004/ca-2-016-red.md` |
| GREEN | CA-2-016 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-016 passa integralmente | `evidencias/spec-2-004/ca-2-016-green.md` |
| RED | CA-2-017 | criar teste `spec-2-004-ca-2-017` com fixture específica e executar API/UI descrita em CA-2-017 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-004/ca-2-017-red.md` |
| GREEN | CA-2-017 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-017 passa integralmente | `evidencias/spec-2-004/ca-2-017-green.md` |
| RED | CA-2-018 | criar teste `spec-2-004-ca-2-018` com fixture específica e executar API/UI descrita em CA-2-018 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-004/ca-2-018-red.md` |
| GREEN | CA-2-018 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-018 passa integralmente | `evidencias/spec-2-004/ca-2-018-green.md` |
| RED | CA-2-019 | criar teste `spec-2-004-ca-2-019` com fixture específica e executar API/UI descrita em CA-2-019 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-004/ca-2-019-red.md` |
| GREEN | CA-2-019 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-019 passa integralmente | `evidencias/spec-2-004/ca-2-019-green.md` |
| RED | CA-2-020 | criar teste `spec-2-004-ca-2-020` com fixture específica e executar API/UI descrita em CA-2-020 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-004/ca-2-020-red.md` |
| GREEN | CA-2-020 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-020 passa integralmente | `evidencias/spec-2-004/ca-2-020-green.md` |
| REFACTOR/REGRESSÃO | todos | reexecutar todos os testes GREEN da SPEC-2-004, build/lint e roteiro integrado da Fase 1 | zero regressão e estado final consistente | `evidencias/spec-2-004/regressao.md` |

**Dados/fixtures:** duas contas fictícias (admin/operador), três empresas, cinco contatos e oito oportunidades cobrindo ativo, ganho, perda, incompleto, vencido e exceção; nenhum dado pessoal real antes de G4.  
**Caminhos de erro obrigatórios:** vazio, inválido, duplicado, não autorizado, usuário inativo, concorrência, timeout/repetição e rollback.  
**Evidência exigida:** log do teste/QA, resposta de API quando aplicável, captura do fluxo, diff auditável e aceite humano por CA.

## Tasks vinculadas

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|
| T2.21 | Implementar e provar CA-2-016 — usuário autorizado cria rascunho com valor, validade, responsável e resumo válidos | Engenharia de produto | SPEC-2-004 | CA-2-016 passa integralmente, com estado final válido | CA-2-016 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-004/ca-2-016.md | SPEC-2-000 aceita; regra configurada com fixture e ativação real reservada à consultora/cliente | ☐ Planejada |
| T2.22 | Implementar e provar CA-2-017 — emissão congela a versão; mudança posterior cria número sequencial sem sobrescrever histórico | Engenharia de produto | SPEC-2-004 | CA-2-017 passa integralmente, com estado final válido | CA-2-017 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-004/ca-2-017.md | T2.21 concluída | ☐ Planejada |
| T2.23 | Implementar e provar CA-2-018 — status inválido, valor negativo, validade passada ou emissão concorrente são bloqueados atomicamente | Engenharia de produto | SPEC-2-004 | CA-2-018 passa integralmente, com estado final válido | CA-2-018 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-004/ca-2-018.md | T2.22 concluída | ☐ Planejada |
| T2.24 | Implementar e provar CA-2-019 — aceite ou recusa registra ator, data, canal, observação e mantém a decisão como humana | Engenharia de produto | SPEC-2-004 | CA-2-019 passa integralmente, com estado final válido | CA-2-019 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-004/ca-2-019.md | T2.23 concluída | ☐ Planejada |
| T2.25 | Implementar e provar CA-2-020 — todo dia às 08:00 no fuso America/Sao_Paulo, proposta vencida aparece na fila do responsável sem alterar resultado comercial | Engenharia de produto | SPEC-2-004 | CA-2-020 passa integralmente, com estado final válido | CA-2-020 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-004/ca-2-020.md | T2.24 concluída | ☐ Planejada |

## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
