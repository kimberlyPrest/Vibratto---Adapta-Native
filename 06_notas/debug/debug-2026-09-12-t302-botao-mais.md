# Debug — 2026-09-12 — T3.02 botão "Mais ⌄" não abre

**Sintoma relatado:** "o botão mais no canto inferior direito deveria abrir? clico mais nada acontece" (print da tela Oportunidades).

**Reprodução:** browser real no preview — clique no botão "Mais ⌄" do card não abria o menu.

**Causa raiz:** `useEffect` global em Opportunities.tsx registra `document.addEventListener('click', fechar)` que fecha o menu em QUALQUER clique — incluindo o clique no próprio botão que abre. O menu abria e fechava no mesmo evento (bubbling: o clique do botão sobe até document e o listener zera `menuAberto`).

**Correção (v0.0.415):** `ev.stopPropagation()` no onClick do botão "Mais ⌄" — o clique não chega mais ao listener global.

**Verificação:** browser real — clique programático no botão do último card: menu abriu com 4 itens (Consulta 360º, Proposta, Tarefas, Formulário). QA verde v0.0.415.

**Gate:** aguardando_teste_humano (T3.02 continua no portão).
