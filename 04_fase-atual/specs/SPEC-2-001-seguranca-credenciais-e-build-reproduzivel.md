# SPEC-2-001 — Segurança de credenciais e build reproduzível

**Fase:** 2  
**Status:** planejada  
**Dono:** Engenharia/Segurança  
**Origem no escopo:** Escopo definitivo §9 (G4 — Dados) e delta da Fase 2 (EV-001/EV-002)  
**Degrau da solução:** reuso do que existe no repo — endurecer o snapshot exportado antes de evoluir o produto

## Resultado observável

O CRM possui provisionamento sem senha fixa, login sem autopreenchimento, dependências instaláveis e pipeline local reproduzível antes de receber dados reais.

## Limites e dependências

- **Inclui:** secrets por ambiente; remoção de credenciais versionadas; lockfile; exportação íntegra do código; build, lint, typecheck e testes reais; política de fixtures.
- **Fora de escopo:** publicação em produção e conexão de integrações externas.
- **Entradas e pré-condições:** código Fase 1 v0.0.84 exportado; variáveis secretas disponíveis no ambiente de implantação.
- **Saídas/artefatos:** snapshot seguro, lockfile, comandos verdes e checklist de rotação.
- **Dependências e responsáveis:** dono da SPEC implementa; consultora homologa regra; cliente executa prova humana.
- **Risco e plano B:** Se o Skip não expuser secrets às migrations, criar conta inicial por rotina administrativa one-shot, nunca reintroduzir senha fixa.
- **Rollback ou reversão:** reverter somente a migration corretiva e manter produção bloqueada; credenciais expostas devem permanecer rotacionadas.

## Fluxo e regras

1. Validar pré-condições, permissões e fixture isolada.
2. Executar o caminho principal e persistir o resultado observável.
3. Exercitar vazio, limite, permissão negada, concorrência e reversão.
4. Integrar ao histórico/auditoria e demonstrar no fluxo 360º.
5. Registrar evidência por critério e obter aceite humano.

1. Nenhuma senha, token ou segredo pode existir em fonte, migration, fixture ou UI.
2. Conta de demonstração não pode estar ativa em produção.
3. Falha de secret obrigatório interrompe provisionamento de forma explícita.

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

- [ ] **CA-2-001:** busca automatizada no snapshot e no histórico novo retorna zero senhas, tokens ou chaves fixas utilizáveis.
- [ ] **CA-2-002:** login não contém ação ou valor que preencha senha; conta com active=false falha na autenticação server-side.
- [ ] **CA-2-003:** exportação contém todos os imports locais; npm ci, typecheck, build, lint e suíte real terminam com código zero no Node declarado.
- [ ] **CA-2-004:** ausência de secret obrigatório interrompe provisionamento sem criar conta parcial; rotação exige valor diferente do exposto.
- [ ] **CA-2-005:** antes de produção, consulta reproduzível confirma zero contas/fixtures ativas e zero seeds de demonstração no denominador real.

## TDD da SPEC

| Etapa | CA | Comando/ação verificável | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | CA-2-001 | executar rg/gitleaks sobre crm, migrations, fixtures e diff da branch antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-001/ca-2-001-red.md` |
| GREEN | CA-2-001 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-001 passa integralmente | `evidencias/spec-2-001/ca-2-001-green.md` |
| RED | CA-2-002 | inspecionar bundle e tentar autenticar conta active=false pela API antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-001/ca-2-002-red.md` |
| GREEN | CA-2-002 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-002 passa integralmente | `evidencias/spec-2-001/ca-2-002-green.md` |
| RED | CA-2-003 | executar npm ci, tsc -b, build, lint e suíte real no Node declarado; conferir imports locais antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-001/ca-2-003-red.md` |
| GREEN | CA-2-003 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-003 passa integralmente | `evidencias/spec-2-001/ca-2-003-green.md` |
| RED | CA-2-004 | executar migration sem secret e com secret novo em banco vazio; comparar ausência de estado parcial antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-001/ca-2-004-red.md` |
| GREEN | CA-2-004 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-004 passa integralmente | `evidencias/spec-2-001/ca-2-004-green.md` |
| RED | CA-2-005 | consultar contas, fixtures e seeds no ambiente candidato à produção; exigir zero ativos no denominador real antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-001/ca-2-005-red.md` |
| GREEN | CA-2-005 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-005 passa integralmente | `evidencias/spec-2-001/ca-2-005-green.md` |
| REFACTOR/REGRESSÃO | todos | reexecutar todos os testes GREEN da SPEC-2-001, build/lint e roteiro integrado da Fase 1 | zero regressão e estado final consistente | `evidencias/spec-2-001/regressao.md` |

**Dados/fixtures:** duas contas fictícias (admin/operador), três empresas, cinco contatos e oito oportunidades cobrindo ativo, ganho, perda, incompleto, vencido e exceção; nenhum dado pessoal real antes de G4.  
**Caminhos de erro obrigatórios:** vazio, inválido, duplicado, não autorizado, usuário inativo, concorrência, timeout/repetição e rollback.  
**Evidência exigida:** log do teste/QA, resposta de API quando aplicável, captura do fluxo, diff auditável e aceite humano por CA.

## Tasks vinculadas

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|
| T2.06 | Implementar e provar CA-2-001 — busca automatizada no snapshot e no histórico novo retorna zero senhas, tokens ou chaves fixas utilizáveis | Engenharia/Segurança | SPEC-2-001 | CA-2-001 passa integralmente, com estado final válido | CA-2-001 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-001/ca-2-001.md | SPEC-2-000 aceita; regra configurada com fixture e ativação real reservada à consultora/cliente | ☐ Planejada |
| T2.07 | Implementar e provar CA-2-002 — login não contém ação ou valor que preencha senha; conta com active=false falha na autenticação server-side | Engenharia/Segurança | SPEC-2-001 | CA-2-002 passa integralmente, com estado final válido | CA-2-002 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-001/ca-2-002.md | T2.06 concluída | ☐ Planejada |
| T2.08 | Implementar e provar CA-2-003 — exportação contém todos os imports locais; npm ci, typecheck, build, lint e suíte real terminam com código zero no Node declarado | Engenharia/Segurança | SPEC-2-001 | CA-2-003 passa integralmente, com estado final válido | CA-2-003 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-001/ca-2-003.md | T2.07 concluída | ☐ Planejada |
| T2.09 | Implementar e provar CA-2-004 — ausência de secret obrigatório interrompe provisionamento sem criar conta parcial; rotação exige valor diferente do exposto | Engenharia/Segurança | SPEC-2-001 | CA-2-004 passa integralmente, com estado final válido | CA-2-004 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-001/ca-2-004.md | T2.08 concluída | ☐ Planejada |
| T2.10 | Implementar e provar CA-2-005 — antes de produção, consulta reproduzível confirma zero contas/fixtures ativas e zero seeds de demonstração no denominador real | Engenharia/Segurança | SPEC-2-001 | CA-2-005 passa integralmente, com estado final válido | CA-2-005 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-001/ca-2-005.md | T2.09 concluída | ☐ Planejada |

## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
