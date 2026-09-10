# SPEC-2-000 — Remediação verificável dos débitos da Fase 1

**Fase:** 2  
**Status:** planejada — primeira unidade obrigatória  
**Dono:** Engenharia/Segurança  
**Origem no escopo:** auditorias F1 de 2026-09-10, check-fase-1 com ressalvas e delta da Fase 2  
**Degrau da solução:** reuso do que existe no repo — completar o contrato canônico antes de ampliar o fluxo

## Resultado observável

O estado herdado fica seguro, portável e aderente aos critérios canônicos pendentes, sem apagar o histórico nem fabricar evidência RED retroativa.

## Limites e dependências

- **Inclui:** campos comerciais pendentes; decisão de modelagem de Empresa; exportação segura server-side; auditoria de negação/delete; portabilidade e retenção.
- **Fora de escopo:** novas regras de qualificação, proposta, SLA, handoff e dashboard.
- **Entradas:** snapshot v0.0.84, SPECs canônicas F1 e relatórios de auditoria.
- **Saídas:** migrations corretivas, testes, decisão registrada e evidências CA-2-036..040.
- **Risco/plano B:** se o Skip impedir endpoint server-side, exportação permanece desabilitada; nunca manter versão vulnerável.
- **Rollback:** migrations corretivas versionadas; rotação de credencial é irreversível por segurança.

## Fluxo e regras

1. Reproduzir cada déficit contra o snapshot e registrar RED.
2. Aplicar correção mínima sem reescrever migrations já aplicadas no ambiente.
3. Executar instalação limpa, RBAC, concorrência, exportação maliciosa e auditoria.
4. Demonstrar os cinco critérios e só então abrir SPEC-2-001/002.

## Checklist de execução

- [ ] RED real anexado por CA.
- [ ] Migration corretiva reversível ou irreversibilidade justificada.
- [ ] Testes automatizados e prova de API anexados.
- [ ] Dados pessoais/fixtures ausentes da produção.
- [ ] Consultora valida decisão Empresa.
- [ ] Aceite humano registrado.

## Critérios de aceite

- [ ] **CA-2-036:** oportunidades possuem origem, tags, responsável, prioridade, score, serviço, status e data de entrada, com validação e auditoria conforme contrato canônico F1.
- [ ] **CA-2-037:** empresa é entidade relacional própria ou decisão de manter campo textual está registrada com impacto, migração e aceite da consultora.
- [ ] **CA-2-038:** CSV neutraliza células iniciadas por =, +, - e @; cancelamento, negação e falha de exportação geram evento append-only.
- [ ] **CA-2-039:** exportação de dados pessoais passa por endpoint server-side autorizado, com filtros/quantidade recalculados e trilha; acesso direto não contorna o aceite.
- [ ] **CA-2-040:** instalação limpa aplica migrations sem IDs de ambiente; delete é auditado; leitura da auditoria respeita papel e retenção definida.

## TDD da SPEC

| Etapa | CA | Comando/ação verificável | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | CA-2-036 | criar fixture com todos os campos e enviar payloads ausentes/inválidos pela API; executar teste opportunities-contract antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-000/ca-2-036-red.md` |
| GREEN | CA-2-036 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-036 passa integralmente | `evidencias/spec-2-000/ca-2-036-green.md` |
| RED | CA-2-037 | executar migration em cópia e provar relação ou abrir decisão assinada com consulta de impacto antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-000/ca-2-037-red.md` |
| GREEN | CA-2-037 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-037 passa integralmente | `evidencias/spec-2-000/ca-2-037-green.md` |
| RED | CA-2-038 | exportar valores =1+1, +cmd, -10 e @SUM; cancelar e forçar falha; consultar eventos append-only antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-000/ca-2-038-red.md` |
| GREEN | CA-2-038 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-038 passa integralmente | `evidencias/spec-2-000/ca-2-038-green.md` |
| RED | CA-2-039 | chamar coleção diretamente sem aceite e endpoint com filtros adulterados; comparar resposta e trilha antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-000/ca-2-039-red.md` |
| GREEN | CA-2-039 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-039 passa integralmente | `evidencias/spec-2-000/ca-2-039-green.md` |
| RED | CA-2-040 | aplicar todas as migrations em banco vazio; criar/deletar fixture; consultar auditoria como operator/admin e testar expiração antes da implementação | falha reproduzível demonstra ausência do comportamento | `evidencias/spec-2-000/ca-2-040-red.md` |
| GREEN | CA-2-040 | repetir a mesma prova após a implementação mínima | comportamento de CA-2-040 passa integralmente | `evidencias/spec-2-000/ca-2-040-green.md` |
| REFACTOR/REGRESSÃO | todos | reexecutar todos os testes GREEN da SPEC-2-000, build/lint e roteiro integrado da Fase 1 | zero regressão e estado final consistente | `evidencias/spec-2-000/regressao.md` |


## Tasks vinculadas

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|
| T2.01 | Implementar e provar CA-2-036 — oportunidades possuem origem, tags, responsável, prioridade, score, serviço, status e data de entrada, com validação e auditoria conforme contrato canônico F1 | Engenharia/Segurança | SPEC-2-000 | CA-2-036 passa integralmente, com estado final válido | CA-2-036 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-000/ca-2-036.md | Fase 1 encerrada e branch limpa | ☐ Planejada |
| T2.02 | Implementar e provar CA-2-037 — empresa é entidade relacional própria ou decisão de manter campo textual está registrada com impacto, migração e aceite da consultora | Engenharia/Segurança | SPEC-2-000 | CA-2-037 passa integralmente, com estado final válido | CA-2-037 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-000/ca-2-037.md | T2.01 concluída | ☐ Planejada |
| T2.03 | Implementar e provar CA-2-038 — CSV neutraliza células iniciadas por =, +, - e @; cancelamento, negação e falha de exportação geram evento append-only | Engenharia/Segurança | SPEC-2-000 | CA-2-038 passa integralmente, com estado final válido | CA-2-038 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-000/ca-2-038.md | T2.02 concluída | ☐ Planejada |
| T2.04 | Implementar e provar CA-2-039 — exportação de dados pessoais passa por endpoint server-side autorizado, com filtros/quantidade recalculados e trilha; acesso direto não contorna o aceite | Engenharia/Segurança | SPEC-2-000 | CA-2-039 passa integralmente, com estado final válido | CA-2-039 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-000/ca-2-039.md | T2.03 concluída | ☐ Planejada |
| T2.05 | Implementar e provar CA-2-040 — instalação limpa aplica migrations sem IDs de ambiente; delete é auditado; leitura da auditoria respeita papel e retenção definida | Engenharia/Segurança | SPEC-2-000 | CA-2-040 passa integralmente, com estado final válido | CA-2-040 — RED reproduz falha; GREEN prova comportamento; regressão preserva fluxo anterior | evidencias/spec-2-000/ca-2-040.md | T2.04 concluída | ☐ Planejada |


## Emendas

| Data | Origem | Micro-spec/task | Motivo |
|---|---|---|---|
