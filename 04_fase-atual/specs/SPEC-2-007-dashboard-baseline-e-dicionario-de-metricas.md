# SPEC-2-007 — Dashboard, baseline e dicionário de métricas

**Fase:** 2  
**Status:** planejada  
**Dono:** Dados/Produto  
**Origem no escopo:** Escopo definitivo §5 (Indicadores) e §9 (G3 — Métricas)  
**Degrau da solução:** reuso do que existe no repo — calcular métricas sobre timestamps e eventos auditados

## Resultado observável

A cliente reproduz cobertura, primeira resposta, tempo por etapa, conversão e perdas a partir de definições versionadas, com cobertura incompleta visível.

## Limites e dependências

- **Inclui:** dicionário; período; denominadores; filtros; cobertura; baseline; drill-down; exportação agregada.
- **Fora de escopo:** atribuição causal de receita/margem, previsão por IA e benchmark externo.
- **Entradas e pré-condições:** eventos da Fase 1 e desta fase; período e definições aprovados; amostra identificada.
- **Saídas/artefatos:** dicionário versionado, baseline congelado, dashboard e consulta reproduzível.
- **Dependências e responsáveis:** dono da SPEC implementa; consultora homologa regra; cliente executa prova humana.
- **Risco e plano B:** Baixo volume torna taxa instável; mostrar N, cobertura e intervalo, sem conclusões causais.
- **Rollback ou reversão:** reverter versão do dicionário e recalcular nova visão sem apagar baseline congelado.

## Fluxo e regras

1. Validar pré-condições, permissões e fixture isolada.
2. Executar o caminho principal e persistir o resultado observável.
3. Exercitar vazio, limite, permissão negada, concorrência e reversão.
4. Integrar ao histórico/auditoria e demonstrar no fluxo 360º.
5. Registrar evidência por critério e obter aceite humano.

1. Toda métrica declara numerador, denominador, relógio, exclusões e fuso.
2. Ausência de dado reduz cobertura; não sai do denominador sem justificativa versionada.
3. Baseline congelado é imutável; correção gera nova versão comparável.

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

- [ ] **CA-2-031:** dicionário registra fórmula, fonte, evento inicial/final, fuso, exclusões e dono para cada métrica.
- [ ] **CA-2-032:** baseline é calculado para período explícito, congelado com versão e reproduzível pela consulta de origem.
- [ ] **CA-2-033:** dashboard exibe leads por origem/período, oportunidades por etapa, primeira resposta, tempo por etapa, propostas/ciclo, conversão, perdas e filas com N e filtros consistentes.
- [ ] **CA-2-034:** dado ausente aparece como cobertura incompleta e não é removido silenciosamente do denominador.
- [ ] **CA-2-035:** drill-down e exportação agregada correspondem aos números exibidos, neutralizam fórmulas e respeitam RBAC/LGPD.

## TDD da SPEC

| Etapa | CA | Comando/ação verificável | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | CA-2-031 | criar teste `spec-2-007-ca-2-031` com fixture específica e executar API/UI descrita em CA-2-031 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-007/ca-2-031-red.md` |
| GREEN | CA-2-031 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-031 passa integralmente | `evidencias/spec-2-007/ca-2-031-green.md` |
| RED | CA-2-032 | criar teste `spec-2-007-ca-2-032` com fixture específica e executar API/UI descrita em CA-2-032 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-007/ca-2-032-red.md` |
| GREEN | CA-2-032 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-032 passa integralmente | `evidencias/spec-2-007/ca-2-032-green.md` |
| RED | CA-2-033 | criar teste `spec-2-007-ca-2-033` com fixture específica e executar API/UI descrita em CA-2-033 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-007/ca-2-033-red.md` |
| GREEN | CA-2-033 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-033 passa integralmente | `evidencias/spec-2-007/ca-2-033-green.md` |
| RED | CA-2-034 | criar teste `spec-2-007-ca-2-034` com fixture específica e executar API/UI descrita em CA-2-034 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-007/ca-2-034-red.md` |
| GREEN | CA-2-034 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-034 passa integralmente | `evidencias/spec-2-007/ca-2-034-green.md` |
| RED | CA-2-035 | criar teste `spec-2-007-ca-2-035` com fixture específica e executar API/UI descrita em CA-2-035 antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-007/ca-2-035-red.md` |
| GREEN | CA-2-035 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-035 passa integralmente | `evidencias/spec-2-007/ca-2-035-green.md` |
| REFACTOR/REGRESSÃO | todos | reexecutar todos os testes GREEN da SPEC-2-007, build/lint e roteiro integrado da Fase 1 | zero regressão e estado final consistente | `evidencias/spec-2-007/regressao.md` |

**Dados/fixtures:** duas contas fictícias (admin/operador), três empresas, cinco contatos e oito oportunidades cobrindo ativo, ganho, perda, incompleto, vencido e exceção; nenhum dado pessoal real antes de G4.  
**Caminhos de erro obrigatórios:** vazio, inválido, duplicado, não autorizado, usuário inativo, concorrência, timeout/repetição e rollback.  
**Evidência exigida:** log do teste/QA, resposta de API quando aplicável, captura do fluxo, diff auditável e aceite humano por CA.

## Tasks vinculadas

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|
| T2.36 | Implementar e provar CA-2-031 — dicionário registra fórmula, fonte, evento inicial/final, fuso, exclusões e dono para cada métrica | Dados/Produto | SPEC-2-007 | CA-2-031 passa integralmente, com estado final válido | CA-2-031 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-007/ca-2-031.md | SPEC-2-000 aceita; regra configurada com fixture e ativação real reservada à consultora/cliente | ☐ Planejada |
| T2.37 | Implementar e provar CA-2-032 — baseline é calculado para período explícito, congelado com versão e reproduzível pela consulta de origem | Dados/Produto | SPEC-2-007 | CA-2-032 passa integralmente, com estado final válido | CA-2-032 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-007/ca-2-032.md | T2.36 concluída | ☐ Planejada |
| T2.38 | Implementar e provar CA-2-033 — dashboard exibe leads por origem/período, oportunidades por etapa, primeira resposta, tempo por etapa, propostas/ciclo, conversão, perdas e filas com N e filtros consistentes | Dados/Produto | SPEC-2-007 | CA-2-033 passa integralmente, com estado final válido | CA-2-033 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-007/ca-2-033.md | T2.37 concluída | ☐ Planejada |
| T2.39 | Implementar e provar CA-2-034 — dado ausente aparece como cobertura incompleta e não é removido silenciosamente do denominador | Dados/Produto | SPEC-2-007 | CA-2-034 passa integralmente, com estado final válido | CA-2-034 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-007/ca-2-034.md | T2.38 concluída | ☐ Planejada |
| T2.40 | Implementar e provar CA-2-035 — drill-down e exportação agregada correspondem aos números exibidos, neutralizam fórmulas e respeitam RBAC/LGPD | Dados/Produto | SPEC-2-007 | CA-2-035 passa integralmente, com estado final válido | CA-2-035 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-007/ca-2-035.md | T2.39 concluída | ☐ Planejada |

## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
