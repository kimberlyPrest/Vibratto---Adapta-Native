# Changelog — CRM Vibratto

## [0.0.401] — 2026-09-12 — Canais TikTok + Página de captura (correção pós-teste)

### Adicionado

- **Canais novos na atribuição granular**: `tiktok` e `pagina_captura` (pedido da cliente, 12/09). Migration 0125 (atribuição direta `field.values` — técnica que persiste no JSVM; `.set()` não persiste, lição reforçada) + UI Opportunities com os 12 canais: Instagram, LinkedIn, TikTok, WhatsApp, Site, Google, Página de captura, Evento, Indicação, Tráfego pago, Parceiro, Outro.
- **Uso**: página de captura com campanhas de Google Ads/Meta → Canal = Página de captura, Campanha = "Google Ads — CFO as a Service", Conteúdo = anúncio específico. Indicação de parceiro que chega no WhatsApp → Canal = Parceiro, Origem específica = "WhatsApp direto — [nome do parceiro]".
- **Provas por API**: create com canal=tiktok → 200; create com canal=pagina_captura + campanha → 200; valor inválido → 400. Limpeza das provas (migration 0126) + remoção da coleção de diagnóstico temporária.
- **Migrations queimadas sem rodar** (0116–0118, 0120, 0122, 0124): removidas do working tree após diagnóstico — o validador do pipeline rejeita remove+add de select sem values intermediário; versões finais: 0125 (fix) e 0126 (limpeza).

## [0.0.380] — 2026-09-12 — T3.01 CONCLUÍDA (teste humano aprovado) — FASE 3 ABERTA

### Concluído

- CA-3-001 fechado: atribuição granular de origem (canal → origem específica → campanha → conteúdo) + motivo de ganho estruturado obrigatório. Teste humano aprovado pela cliente (2026-09-12 22:53 — "Agora, sim, TESTE REALIZADO").
- **Migration 0110**: campos novos em `negocios` — `canal`, `origem_especifica`, `campanha`, `conteudo`, `motivo_ganho` (preco, escopo, relacionamento, urgencia, indicacao_interna, outro), `motivo_ganho_detalhe`. Campo `origem` antigo preservado.
- **Hook `ganho_motivo_rules.js`** (request hook, dono único): ganho sem motivo → 400; motivo inválido → 400; `outro` sem detalhe → 400; motivo registrado não pode ser removido; reabertura limpa motivo com trilha preservada.
- **UI Opportunities**: formulário com os 4 níveis de atribuição + bloco de ganho com motivo obrigatório e detalhe condicional.
- **Dashboard**: blocos novos `leads_por_canal` e `ganhos_por_motivo` + drill-down + export CSV.
- **Caso real registrado**: Felicidade Collective — BPO Financeiro e Tesouraria, R$ 8.336,11/mês, ganha 07/2026, canal indicação, motivo relacionamento, handoff único criado.
- **Correções no caminho (autorizadas pela cliente)**:
  - v0.0.374: home — link "Abrir oportunidades" + card Pipeline Comercial clicável.
  - v0.0.379: home — card Base de Contatos clicável.
  - v0.0.375–0.0.378: `servico` ganha **Tesouraria** — migration 0112/0113 + fix da causa raiz no hook `commercial_contract.js` (SERVICES hardcoded).
- **Limpezas**: migrations 0111, 0114, 0119, 0126 — base final: 3 negócios reais.
- **Revalidação independente**: ganho sem motivo 400 · com motivo 200 · inválido 400 · tesouraria 200 · Felicidade íntegra · dashboard consistente.
