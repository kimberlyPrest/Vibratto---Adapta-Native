# Changelog — CRM Vibratto

## [0.0.380] — 2026-09-12 — T3.01 CONCLUÍDA (teste humano aprovado) — FASE 3 ABERTA

### Concluído

- CA-3-001 fechado: atribuição granular de origem (canal → origem específica → campanha → conteúdo) + motivo de ganho estruturado obrigatório. Teste humano aprovado pela cliente (2026-09-12 22:53 — "Agora, sim, TESTE REALIZADO").
- **Migration 0110**: campos novos em `negocios` — `canal` (select: instagram, linkedin, whatsapp, site, google, evento, indicacao, trafego_pago, parceiro, outro), `origem_especifica`, `campanha`, `conteudo`, `motivo_ganho` (preco, escopo, relacionamento, urgencia, indicacao_interna, outro), `motivo_ganho_detalhe`. Campo `origem` antigo preservado (compatibilidade dashboard/filtro).
- **Hook `ganho_motivo_rules.js`** (request hook, dono único): ganho sem motivo → 400; motivo inválido → 400; `outro` sem detalhe → 400; motivo registrado não pode ser removido; reabertura limpa motivo com trilha preservada; tentativa negada em log estruturado.
- **UI Opportunities**: formulário com os 4 níveis de atribuição + bloco de ganho com motivo obrigatório e detalhe condicional; validação client-side espelhando o hook.
- **Dashboard**: blocos novos `leads_por_canal` e `ganhos_por_motivo` + drill-down + export CSV (neutralização OWASP mantida).
- **Caso real registrado**: Felicidade Collective — BPO Financeiro e Tesouraria, R$ 8.336,11/mês, ganha 07/2026, canal indicação, motivo relacionamento, handoff único criado. Primeiro dado real de atribuição do funil.
- **Correções no caminho (autorizadas pela cliente)**:
  - v0.0.374: home — link "Abrir oportunidades" + card Pipeline Comercial clicável → /oportunidades (gap de navegação apontado no teste).
  - v0.0.379: home — card Base de Contatos clicável → /contatos (mesmo padrão).
  - v0.0.375–0.0.378: `servico` ganha **Tesouraria** (pedido da cliente: BPO Financeiro, Tesouraria, Controladoria, CFO as a Service) — migration 0112/0113 (schema) + fix da causa raiz no hook `commercial_contract.js` (lista SERVICES hardcoded sem tesouraria bloqueava o valor; provado: PATCH tesouraria 200, inválido 400).
- **Limpezas**: migration 0111 (fixtures de prova T301) e 0114 (negócio de prova da revalidação) — base final: 3 negócios reais (Proposta BPO, Proposta CFO, Felicidade Collective).
- **Revalidação independente (v0.0.378–0.0.380)**: ganho sem motivo 400 · ganho com motivo 200 · motivo inválido 400 · servico=tesouraria 200 · valor inválido 400 · Felicidade íntegra · dashboard consistente (leads_por_canal: indicacao 1, linkedin 1, sem_canal 1; ganhos_por_motivo: relacionamento 2).
- **FASE 3: 1 task concluída.** Próxima leva: SPEC-3-001 (formulários inteligentes por solução — base no documento "Onda 3" da cliente, confirmado complementar).
