# SPEC-3-010 — Painel por papel (camada CEO) + metas + comparativo de período

**Task:** T3.10 (Fase 3 — leva 10)
**Base:** backlog Etapa 3, itens 1–3 (painel por papel; metas e realizado + comparativo automático; MRR/receita nova/ticket médio) + decisão da CEO (13/09): "faça a sugestão avançada, com vários KPIs, e vamos melhorando à medida que identificarmos oportunidades".
**Princípio:** a Direção abre o CRM e vê a SAÚDE DO NEGÓCIO em uma tela — número, meta, realizado e variação vs. período anterior. Papéis menores veem recortes do mesmo painel, não painéis paralelos.

## 1. Objetivo

Painel de Direção (CEO) com 12 KPIs organizados em 4 grupos (Aquisição, Pipeline, Financeiro, Operação), cada um com: valor do período, meta (quando configurada), % da meta e variação % vs. período anterior equivalente. Comercial vê recorte de pipeline/aquisição; Controladoria vê recorte financeiro/operação; Administração vê o painel atual (dashboard comercial completo).

## 2. Escopo

### 2.1 Coleção `metas_indicadores` (nova)
- chave (text, único — ex.: `novos_clientes_cfo_mensal`), papel (select: direcao|comercial|controladoria|administracao), valor_meta (number), periodicidade (select: mensal|semanal|trimestral), ativo (bool), descricao (text), created/updated.
- Regras: CRUD admin-only (create/update/delete `@request.auth.role = 'admin'`); leitura autenticada.
- Seed inicial (migration): metas da CEO para Q3/Q4 2026 — novos_clientes_cfo_mensal = 2 (6 até dez/2026), diagnosticos_semanais = 1.5 (média de 1–2), horas_venda_semanais = 4. Valores EDITÁVEIS pelo admin sem código.

### 2.1b Campo `recorrencia` em `negocios` (migration)
- select: `mensal` | `unico`, padrão `mensal`, aplicável a todos os serviços (BPO/Tesouraria/Controladoria ignoram o campo na prática — regra fixa).
- No ganho de negócio de Consultoria ou CFO as a Service, o formulário de decisão pergunta a recorrência (UI: select no modal de ganho quando serviço ∈ {consultoria, cfo_as_a_service}).
- Negócios ganhos existentes: migration preenche `mensal` (premissa atual — revisável caso a CEO saiba de contrato único).

### 2.2 Endpoint `GET /backend/v1/painel/{papel}` (hook `painel_papel_endpoint.js`)
- Papéis: direcao | comercial | controladoria | administracao. Papel inválido → 400. Sem auth → 401.
- **Período**: mês corrente por padrão; `?periodo_inicio=&periodo_fim=` opcional (mesma validação do dashboard comercial).
- **Comparativo automático**: para cada KPI, calcula também o período anterior de MESMA duração (ex.: mês atual 01–30/09 → anterior 01–30/08) e retorna `valor_anterior` + `variacao_pct` ((atual−anterior)/anterior×100; anterior=0 → null).
- **KPIs por papel** (todos calculados server-side a partir das coleções existentes — nenhuma regra nova inventada):
  - **Direção (12 KPIs)**:
    - Aquisição: novos negócios no período; leads de entrada (leads_entrada); taxa lead→negócio.
    - Pipeline: oportunidades ativas; propostas em aberto (n + valor total); propostas paradas >10 dias (usa config existente); conversão etapa→ganho.
    - Financeiro: MRR contratado (soma valor mensal dos negócios ganhos ativos — campo valor interpretado como mensal, padrão do CRM); receita nova no período (soma valor dos ganhos do período); ticket médio (receita nova ÷ n ganhos).
    - Operação: tarefas vencidas; oportunidades paradas; primeira resposta p50; tempo médio de decisão.
  - **Comercial (6)**: novos negócios, propostas em aberto (n+valor), propostas paradas, conversão, primeiras respostas, tarefas vencidas.
  - **Controladoria (6)**: MRR, receita nova, ticket médio, oportunidades paradas, tempo por etapa, clientes ativos (n negócios ganhos não arquivados).
  - **Administração**: redireciona para o dashboard comercial existente (sem duplicação).
- **Metas**: para cada KPI com meta ativa daquele papel, retorna `meta`, `pct_meta` (valor/meta×100) e `atingida` (bool). KPI sem meta retorna meta:null — o painel mostra o número sem régua.
- **Cobertura**: blocos sem dado suficiente retornam aviso (padrão do dashboard comercial); falha de fonte não some com o painel.
- Somente leitura. Nada é alterado comercialmente (guard T2.18 intacto).

### 2.3 UI — página `/painel-direcao` (e recortes por papel)
- **Painel de Direção**: 4 grupos em grade; cada KPI é um card no padrão harmonizado (ícone preto + glifo dourado, título bold, descrição cinza): valor grande, linha de meta ("Meta: X · 67% atingida" com barra de progresso dourada), linha de variação ("▲ +12% vs. mês anterior" verde / "▼ −8%" vermelho / cinza quando null).
- **Seletor de período**: mês corrente (padrão) ou período customizado.
- **Cards de módulo na home**: "Painel de Direção" (admin) e "Painel Comercial"/"Painel Financeiro" (operator vê comercial; controladoria futura com perfil próprio) — todos no padrão harmonizado.
- **Administração**: card existente do dashboard comercial mantido.

### 2.4 Premissa do MRR (decisão da CEO, 13/09)
- Contratos de BPO, Tesouraria e Controladoria têm vigência de **12 meses com renovação automática** — o campo `valor` do negócio é a **mensalidade** do contrato anual recorrente.
- Consequência: para BPO, Tesouraria e Controladoria, MRR = soma direta do `valor` dos negócios ganhos ativos.
- **Consultoria e CFO as a Service podem ter prazo menor que 12 meses** (decisão da CEO, 13/09) — o `valor` desses serviços NÃO é automaticamente mensalidade recorrente. Tratamento: campo novo `recorrencia` (select: mensal | unico, padrão mensal) nos negócios ganhos desses dois serviços; MRR soma o valor apenas se `recorrencia = mensal`; contratos `unico` alimentam "Receita nova" mas não o MRR. BPO/Tesouraria/Controladoria não precisam do campo (regra fixa: sempre mensal).
- O painel exibe a premissa explicitamente: "MRR = mensalidade de contratos de 12 meses renováveis (BPO, Tesouraria, Controladoria) + parcelas recorrentes de Consultoria/CFO marcadas como mensal".

### 2.5 LGPD e auditoria
- Endpoint somente leitura, autenticado; nenhum dado pessoal novo é coletado; metas são dados internos de negócio.
- Auditoria: criação/edição de meta gera evento (padrão existente).

## 3. Fora do escopo
- Relatórios agendados por e-mail (item 6 do backlog — task própria); exportação do painel (drill-down/export já existem no dashboard comercial); metas por usuário individual; projeção/forecast; KPIs de caixa (dependem do sistema financeiro — fora do CRM).

## 4. Critérios de aceite e provas

| ID | Critério | Prova |
|---|---|---|
| CA-3-028 | Painel de Direção retorna 12 KPIs com valor, comparativo (valor_anterior + variacao_pct) e metas quando configuradas | RED: sem auth 401, papel inválido 400; GREEN: fixture com negócios em 2 períodos → KPIs com atual/anterior/variacao corretos (contas conferidas à mão na prova) |
| CA-3-029 | Metas são configuráveis pelo admin sem código e alimentam pct_meta/atingida | RED: operator cria meta → 403; GREEN: admin cria/edita meta → painel reflete pct_meta |
| CA-3-030 | Papéis veem recortes distintos e privacidade por papel | GREEN: /painel/comercial retorna 6 KPIs (não os 12); /painel/controladoria retorna os 6 financeiros/operação |
| CA-3-031 | Comparativo de período é correto para períodos de duração igual | GREEN: período 01–15/09 vs. 16–31/08 (mesma duração) com fixture em ambos → variação correta; período invertido → 400 |

## 5. Riscos e cuidados
- **Interpretação do valor (RESOLVIDA pela CEO 13/09)**: contratos BPO/Tesouraria/Controladoria = 12 meses renováveis automaticamente; `valor` = mensalidade. MRR = soma direta dos ganhos ativos. Aviso de premissa exibido no painel (não é estimativa — é regra de negócio declarada).
- Períodos sem dados: variacao_pct null (não 0%) — zero não é dado.
- Performance: consultas limitadas (20000 registros, padrão dos endpoints existentes).
- Metas seed são EDITÁVEIS — números da CEO, não inventados pelo agente (documentado na migration).
