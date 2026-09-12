# AP-2026-09-13-0850 — agent-browser: clique sintético não dispara handlers de itens de menu React

- Status: candidato
- Escopo: projeto do cliente (CRM Vibratto) — ferramenta de verificação, não produto
- Task/SPEC: T3.04 / SPEC-3-003
- Sinal: clique via agent-browser (click @ref) no item "Timeline" do menu Mais ⌄ fechou o menu mas não abriu o modal; o MESMO clique no item "Consulta 360º" abriu o modal normalmente; clique programático via eval (element.click()) abriu o modal Timeline normalmente.
- Evidência: snapshots do browser durante o teste delegado da T3.04; print artifacts/t304_timeline_aberto.png (modal aberto com 6 eventos em ordem); mesmo padrão observado na T3.03 (modal WhatsApp não abria no automatizado e passou no teste humano).
- Regra reutilizável: falha de abertura de modal via clique sintético do agent-browser NÃO é evidência de bug de produto quando o handler está correto no bundle e a API responde — reproduzir com eval element.click() antes de abrir debug-task; se abrir, registrar como limitação da ferramenta e seguir para o teste humano.
- Quando aplicar: testes automatizados de UI com menus/dropdowns React no agent-browser.
- Quando não aplicar: se o clique programático TAMBÉM falhar, aí sim é bug real — seguir debug-task.
- Confiança: alta — reproduzido 2x (T3.03 e T3.04) com o mesmo desfecho (produto correto).
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto.
