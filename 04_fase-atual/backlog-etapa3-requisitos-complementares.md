# Backlog — Requisitos Complementares da Etapa 3

**Fonte:** documento "CRM Vibratto — Requisitos complementares da etapa 3: camada analítica, colaboração, inteligência artificial e administração" (v1.0, 12/09/2026, autoria Deniane Bezerra).
**Regra de governança:** este backlog é **ADITIVO**. Nada do plano original da Fase 3 (T3.01–T3.06 e demais tasks do documento Onda 3) é deletado ou substituído. Correções são permitidas e registradas no changelog; cada item abaixo só entra em execução como task formal, com SPEC publicada e autorização da CEO, mantendo os portões humanos.

## 1. Cruzamento com o que já existe (verificado no schema e hooks do CRM)

### Já atendido (não reconstruir — apenas evoluir)

| Requisito do documento | Estado atual | Origem |
|---|---|---|
| Registro de auditoria (valor anterior/novo, quem, quando) | Coleção `auditoria`, retenção 365 dias, leitura por papel | T2.05 |
| Exportação em planilha com trilha | Exportação server-side + aceite de uso único + neutralização de fórmulas (=, +, -, @) | T2.04 |
| Detalhamento por clique (drill-down) | Drill-down + export CSV do dashboard | T2.40 |
| Base dos indicadores do painel de direção | Dashboard comercial: negócios por etapa, conversão por etapa, tempo por etapa (ciclo), origem por canal declarado e técnico, motivo de perda, propostas/ciclo, primeira resposta, filas, cobertura — com N e filtros | T2.38 |
| Negócios parados / sem atividade | Fila "oportunidades paradas" + "ações vencidas" + automações diárias (follow-up 3/7 dias, sem próxima ação, parada) em log idempotente | T2.28–30, T3.06 |
| SLA e filas operacionais | Config de SLA por evento/etapa + filas distintas | T2.26–28 |
| Timeline unificada (pré-requisito do §1 do documento) | Timeline 360º consolidando 9 fontes, endpoint somente leitura | T3.04 |
| Perfis básicos | `users.role` (admin/operator) + regras de API por papel em todas as coleções | Fases 1–2 |
| Formulário de entrada (referência do documento) | Porta 2 pronta (T3.02); Porta 1 analisada (SPEC-3-001, aguardando autorização) | T3.02 |

### Faltante (a construir como tasks formais)

**Etapa 3, antes da produção (prioridade alta conforme §7 do documento):**
1. Painel por papel (direção, comercial, controladoria/financeiro, administração) — hoje o painel é único.
2. Metas e realizado + comparação automática com o período anterior em todo indicador numérico (valor absoluto e percentual).
3. Receita recorrente contratada, receita nova no período e ticket médio por linha de solução.
4. Fila de trabalho pessoal (painel lateral com registros atribuídos e pendências do dia).
5. Comentário no registro + menção direta + notificação interna central (comentário grava na timeline).
6. Relatórios salvos em pastas (próprios/compartilhados) + agendamento por e-mail com periodicidade e destinatários.
7. Perfis e permissões completos por módulo + visibilidade por responsável (restrição de visualização).
8. Cópia de segurança automática com teste de restauração documentado.
9. V.ia estágio 1 — resumo de conversa/registro, extração de pendências, sugestão de próximo passo, classificação de assunto (somente time interno).
10. Catálogo de serviços (5 linhas com escopo e faixa de preço padronizados).

**Etapa 4 (por decisão do próprio documento):** V.ia estágio 2 (rascunho revisado por humano), chamados de pós-venda, contratos/assinatura eletrônica (integrar serviço existente), caixa de entrada priorizada pelo funil.
**Etapa 5:** V.ia estágio 3 (condicionado à estabilidade do estágio 2), campanhas, rastreamento de visitantes.
**Não construir:** faturamento e cobrança (função do sistema financeiro em uso).

### Divergências registradas (a confirmar com a CEO)

- O documento lista "Propostas — construir na etapa 4", mas o CRM já possui propostas versionadas com decisão auditada e fila de vencidas (Fase 2 completa). Hipótese: o documento foi escrito sem essa visão; manter o que existe e tratar o item como evolução, não construção.
- O documento cita "cinco linhas de solução" no catálogo; o CRM hoje tem 5 serviços no enum (BPO, Tesouraria, Controladoria, CFO as a Service, Outro) — confirmar correspondência.

## 2. Sequência proposta (respeitando o §7 e o que está em andamento)

1. Porta 1 — formulário de entrada (SPEC-3-001, já analisada, aguardando autorização).
2. Fila de trabalho pessoal + comentários/menções/notificações (esforço baixo, impacto direto na adoção).
3. Painel por papel + metas + comparação de período.
4. Relatórios salvos e agendados por e-mail.
5. Perfis/permissões completos + visibilidade por responsável + backup com teste de restauração.
6. V.ia estágio 1.
7. Catálogo de serviços.

## 3. Decisões pendentes da CEO (§9 do documento)

| Tema | Bloqueia |
|---|---|
| Metas por indicador do painel de direção | Item 2 e 3 da sequência |
| Periodicidade e destinatários dos relatórios agendados | Item 4 |
| Prazo de retenção de leads não convertidos (LGPD) | Item 7 (visibilidade/retenção) e Porta 1 |
| Modelo de IA e fornecedor + tratamento de dados | V.ia estágio 1 |
| Escopo fechado de perguntas do estágio 3 | V.ia estágio 3 (etapa 5) |
| Verificação de disponibilidade do nome/domínio V.ia | Antes de qualquer divulgação externa |


## 4. Pedidos da CEO (aditivo — 13/09)

- **Harmonização visual dos cards** (pedido 13/09, 10:47): os cards de cada task/tela devem seguir o padrão visual dos cards de módulo da home (ícone em quadrado preto com glifo dourado, título bold, descrição cinza, CTA dourado "Abrir X →", fundo bege claro) — referência: prints enviados pela CEO (cards "Pipeline Comercial", "Base de Contatos", "Contas & Empresas"). Aplicável a: cards de oportunidade, cards de módulo, cards de automações e próximos painéis. Entra como task formal com SPEC quando a CEO autorizar; sugestão de posição na sequência: junto do item "Painel por papel" (mesma frente visual).


## 5. Módulo de Operação — Ficha Operacional do Cliente (aditivo — 13/09)

**Fonte:** documento "Ficha Operacional do Cliente — Especificação Dev" v1.0 (12/09/2026, autoria Deniane Bezerra; base: 8 procedimentos operacionais de clientes ativos + 5 roteiros de processo). Pedido da CEO: ficha no CRM com acesso limitado aos analistas; rotinas repetitivas serão automatizadas, mas a ficha permanece como referência operacional.

**Decisão central do documento:** o procedimento operacional deixa de ser documento manual e passa a ser TEXTO GERADO a partir dos parâmetros cadastrados — manutenção no cadastro, não no documento. Um processo único com configurações diferentes por cliente.

**Quebra em levás (recorte por task formal):**
1. **Leva A — Ficha Operacional** (blocos 1–9: identificação/responsáveis, canais de entrada, sistema de gestão, contas bancárias, contas a pagar, faturamento, conciliação, fechamento, pessoas do cliente) + **procedimento gerado** (cap. 3, versionado, exportável, sem credencial).
2. **Leva B — Motor de rotinas** (cap. 4: geração automática de obrigações com calendário de dias úteis, baixa em um toque, baixa em lote, substituição titular→reserva) + **controle de exceções** (cap. 5: 10 exceções com gatilho/prazo/destinatário).
3. **Leva C — Visões** (cap. 6: analista, coordenação, comercial somente leitura) + **implantação** (cap. 7: modelo padrão de etapas) + **permissões** (cap. 8: direção/coordenação/analista/comercial).

**Decisões pendentes da CEO (cap. 11 do documento):** cofre de senhas a adotar; prazos de resposta por cliente (base dos alertas); feriados municipais; parâmetros de conciliação/fechamento (só existem procedimentos de contas a pagar e faturamento); volumes de referência contratados.

**Regra de segurança inegociável (cap. 9):** nenhuma tabela armazena credencial — os campos de cofre guardam apenas o IDENTIFICADOR do item no cofre corporativo.
