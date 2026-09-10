# SPEC-2-003 — Diagnóstico comercial e próximo passo

**Fase:** 2  
**Status:** planejada  
**Dono:** Produto/Operação comercial  
**Origem no escopo:** Escopo definitivo §4 (diagnóstico, responsável e próxima ação)  
**Degrau da solução:** construção mínima — adicionar registro versionado de diagnóstico sobre as entidades existentes

## Resultado observável

Cada oportunidade ativa possui diagnóstico comercial consultável, responsável e próximo passo datado, sem automatizar julgamento humano.

## Limites e dependências

- **Inclui:** dor, contexto, impacto, urgência, stakeholders, restrições, notas e próximo passo.
- **Fora de escopo:** recomendação automática, geração por IA e gravação/transcrição de reuniões.
- **Entradas e pré-condições:** oportunidade ativa e qualificação iniciada ou exceção autorizada.
- **Saídas/artefatos:** diagnóstico versionado, responsável, próxima ação e histórico.
- **Dependências e responsáveis:** dono da SPEC implementa; consultora homologa regra; cliente executa prova humana.
- **Risco e plano B:** Campos excessivos reduzem adoção; permitir configuração de obrigatoriedade, mantendo núcleo mínimo.
- **Rollback ou reversão:** inativar campos adicionais sem excluir versões anteriores.

## Fluxo e regras

1. Validar pré-condições, permissões e fixture isolada.
2. Executar o caminho principal e persistir o resultado observável.
3. Exercitar vazio, limite, permissão negada, concorrência e reversão.
4. Integrar ao histórico/auditoria e demonstrar no fluxo 360º.
5. Registrar evidência por critério e obter aceite humano.

1. Diagnóstico é versão append-only; correção gera nova versão.
2. Oportunidade ativa exige responsável e próxima ação futura ou exceção vigente.
3. Dados ausentes são exibidos como ausentes, nunca inferidos.

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

- [ ] **CA-2-011:** operador cria versão de diagnóstico com núcleo mínimo e vínculo inequívoco à oportunidade.
- [ ] **CA-2-012:** edição preserva versão anterior, ator, data e motivo da atualização.
- [ ] **CA-2-013:** oportunidade ativa persiste responsável e próxima ação futura ou exceção vigente; a fila é responsabilidade da SPEC-2-005.
- [ ] **CA-2-014:** data passada, responsável inativo e texto acima de 5.000 caracteres são rejeitados sem estado parcial.
- [ ] **CA-2-015:** consulta 360º exibe versão atual, histórico e campos ausentes explicitamente.

## TDD da SPEC

| Etapa | CA | Comando/ação verificável | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | CA-2-011 | criar teste `spec-2-003-ca-2-011` com fixture específica e executar API/UI descrita em CA-2-011 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-003/ca-2-011-red.md` |
| GREEN | CA-2-011 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-011 passa integralmente | `evidencias/spec-2-003/ca-2-011-green.md` |
| RED | CA-2-012 | criar teste `spec-2-003-ca-2-012` com fixture específica e executar API/UI descrita em CA-2-012 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-003/ca-2-012-red.md` |
| GREEN | CA-2-012 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-012 passa integralmente | `evidencias/spec-2-003/ca-2-012-green.md` |
| RED | CA-2-013 | criar teste `spec-2-003-ca-2-013` com fixture específica e executar API/UI descrita em CA-2-013 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-003/ca-2-013-red.md` |
| GREEN | CA-2-013 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-013 passa integralmente | `evidencias/spec-2-003/ca-2-013-green.md` |
| RED | CA-2-014 | criar teste `spec-2-003-ca-2-014` com fixture específica e executar API/UI descrita em CA-2-014 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-003/ca-2-014-red.md` |
| GREEN | CA-2-014 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-014 passa integralmente | `evidencias/spec-2-003/ca-2-014-green.md` |
| RED | CA-2-015 | criar teste `spec-2-003-ca-2-015` com fixture específica e executar API/UI descrita em CA-2-015 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-003/ca-2-015-red.md` |
| GREEN | CA-2-015 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-015 passa integralmente | `evidencias/spec-2-003/ca-2-015-green.md` |
| REFACTOR/REGRESSÃO | todos | reexecutar todos os testes GREEN da SPEC-2-003, build/lint e roteiro integrado da Fase 1 | zero regressão e estado final consistente | `evidencias/spec-2-003/regressao.md` |

**Dados/fixtures:** duas contas fictícias (admin/operador), três empresas, cinco contatos e oito oportunidades cobrindo ativo, ganho, perda, incompleto, vencido e exceção; nenhum dado pessoal real antes de G4.  
**Caminhos de erro obrigatórios:** vazio, inválido, duplicado, não autorizado, usuário inativo, concorrência, timeout/repetição e rollback.  
**Evidência exigida:** log do teste/QA, resposta de API quando aplicável, captura do fluxo, diff auditável e aceite humano por CA.

## Tasks vinculadas

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|
| T2.16 | Implementar e provar CA-2-011 — operador cria versão de diagnóstico com núcleo mínimo e vínculo inequívoco à oportunidade | Engenharia de produto | SPEC-2-003 | CA-2-011 passa integralmente, com estado final válido | CA-2-011 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-003/ca-2-011.md | SPEC-2-000 aceita; regra configurada com fixture e ativação real reservada à consultora/cliente | ☐ Planejada |
| T2.17 | Implementar e provar CA-2-012 — edição preserva versão anterior, ator, data e motivo da atualização | Engenharia de produto | SPEC-2-003 | CA-2-012 passa integralmente, com estado final válido | CA-2-012 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-003/ca-2-012.md | T2.16 concluída | ☐ Planejada |
| T2.18 | Implementar e provar CA-2-013 — oportunidade ativa persiste responsável e próxima ação futura ou exceção vigente; a fila é responsabilidade da SPEC-2-005 | Engenharia de produto | SPEC-2-003 | CA-2-013 passa integralmente, com estado final válido | CA-2-013 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-003/ca-2-013.md | T2.17 concluída | ☐ Planejada |
| T2.19 | Implementar e provar CA-2-014 — data passada, responsável inativo e texto acima de 5.000 caracteres são rejeitados sem estado parcial | Engenharia de produto | SPEC-2-003 | CA-2-014 passa integralmente, com estado final válido | CA-2-014 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-003/ca-2-014.md | T2.18 concluída | ☐ Planejada |
| T2.20 | Implementar e provar CA-2-015 — consulta 360º exibe versão atual, histórico e campos ausentes explicitamente | Engenharia de produto | SPEC-2-003 | CA-2-015 passa integralmente, com estado final válido | CA-2-015 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-003/ca-2-015.md | T2.19 concluída | ☐ Planejada |

## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
