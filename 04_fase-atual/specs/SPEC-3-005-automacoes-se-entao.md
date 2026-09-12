# SPEC-3-005 — Automações Se/Então (§12 do doc Onda 3) — leva 6

**Task:** T3.06 (Fase 3 — leva 6)
**Base:** documento "Onda 3 — Conexão, Qualificação e Conversão" (§12 Automação do processo), confirmado pela cliente como complementar em 12/09/2026.
**Princípio central (da cliente):** automatizar o processo para reduzir ao máximo o registro manual — sem automatizar julgamento comercial (regra da Fase 2, mantida).

## 1. Objetivo

As automações desejadas no §12 que JÁ têm fundação pronta no CRM entram em execução real: proposta enviada → follow-up automático; sem resposta → follow-up por SLA; oportunidade sem próxima ação → alerta; oportunidade parada → alerta. O que já existia como fila de leitura (T2.28/T2.30) passa a ter geração automática diária e rastreabilidade de execução.

## 2. Estado atual (inspecionado)

- **Cron diário já existe** (T2.25): `fila_propostas_vencidas_cron.js` roda 08:00 BRT, varre propostas emitidas vencidas e registra na fila — idempotente por proposta+dia.
- **Filas de leitura já existem** (T2.28/T2.30): `/operacional/acoes-vencidas` (próxima ação passada) e `/operacional/paradas` (permanência aberta acima do limite configurável) — calculadas on-demand, sem registro histórico.
- **O que falta**: follow-up automático após proposta enviada; follow-up por SLA quando não há resposta; registro histórico das detecções (hoje a leitura é on-demand e não deixa rastro); alerta visível no painel.

## 3. Escopo (recorte mínimo completo)

### 3.1 Coleção `automacoes_execucoes` (append-only, log de execuções)
- Campos: regra (select: follow_up_proposta | follow_up_sem_resposta | alerta_sem_proxima_acao | alerta_parada), negocio (relation), responsavel (relation), detalhe (json — snapshot mínimo: versão da proposta, dias parado, etc.), dia_referencia (date), created.
- Regras: create apenas server-side (null no createRule); list/view autenticado; delete bloqueado.
- Índice UNIQUE (regra + negocio + dia_referencia) — idempotência por dia, mesmo padrão do cron T2.25.

### 3.2 Cron diário `automacoes_diarias` (08:05 BRT — após o cron de propostas)
- Varre oportunidades ativas (não arquivadas, estágio não-final) e gera execuções:
  - **follow_up_proposta**: proposta emitida há ≥ 3 dias sem decisão (status emitida, created/emitida_em ≥ 3 dias) → registra execução com versão e valor da proposta.
  - **follow_up_sem_resposta**: proposta emitida há ≥ 7 dias sem decisão → registra execução (regra mais severa, substitui a anterior no dia).
  - **alerta_sem_proxima_acao**: oportunidade ativa sem próxima ação futura (proxima_acao_em vazia/passada) → registra execução.
  - **alerta_parada**: permanência aberta na etapa atual acima do limite configurado (reusa `limite_oportunidade_parada_dias` de configuracoes_operacionais) → registra execução com dias parado.
- NUNCA altera resultado comercial (status, estágio, valor) — só registra. Garantia herdada do T2.25.
- Idempotente por regra+negócio+dia (UNIQUE) — reexecução não duplica.

### 3.3 Endpoint de leitura
- `GET /backend/v1/automacoes/execucoes?dia={YYYY-MM-DD}` (auth): execuções do dia agrupadas por regra, com contagem e lista (título da oportunidade, responsável, detalhe). Sem parâmetro dia → hoje.

### 3.4 UI no painel Operacional
- Nova seção "Automações de hoje" no topo do painel: 4 cards (uma por regra) com contagem do dia e lista das oportunidades (clique navega para a oportunidade). Estado vazio explícito: "Nenhuma automação disparou hoje".

## 4. Fora do escopo desta task
- Envio automático de WhatsApp/e-mail (as execuções são insumo para o time agir — o envio depende das integrações de leva futura).
- IA (Fase 4/5).
- Configuração de regras pelo administrador (os limiares 3/7 dias e o limite de parada vêm de constantes/config existentes; parametrização completa é refinamento futuro).

## 5. Critérios de aceite e provas (TDD)

| ID | Critério | Prova |
|---|---|---|
| CA-3-015 | Cron gera execuções corretas e idempotentes | GREEN: fixture com proposta emitida há 5 dias → execução follow_up_proposta no dia; reexecução do mesmo dia não duplica (UNIQUE); proposta com decisão não gera execução; oportunidade final/arquivada não gera |
| CA-3-016 | Alertas de saúde da oportunidade | GREEN: oportunidade ativa sem próxima ação futura → alerta_sem_proxima_acao; permanência aberta acima do limite → alerta_parada com dias; oportunidade saudável não gera nada |
| CA-3-017 | Somente leitura comercial + leitura estruturada | RED: tentativa de create direto na coleção 403 (createRule null); GREEN: cron não altera status/estágio/valor (regressão por contagem de campos); endpoint GET agrupa por regra com contagem |

## 6. Riscos e cuidados
- Cron NUNCA toca campos comerciais (garantia T2.25) — só insere em automacoes_execucoes.
- Idempotência por UNIQUE regra+negócio+dia; corrida benigna logada como warn (padrão T2.25).
- Limiares 3/7 dias como constantes nomeadas no hook (documentadas na SPEC); limite de parada reusa config existente.
- JSVM: lógica inline no callback do cron; datas PB " " → "T"; 0001-01-01 = ausente.
- Fixtures de prova limpas ao final (padrão 0143/0144/0146).

## 7. Evidência esperada
- evidencias/spec-3-005/ca-3-015.md … ca-3-017.md (GREEN por API + regressão)
- Caso real: cron executa no dia e o painel Operacional mostra as automações do dia para as oportunidades reais (Felicidade, Proposta BPO, Proposta CFO).
- Teste humano: Deniane abre o painel Operacional e vê as automações de hoje com contagem e lista (ou delega).
