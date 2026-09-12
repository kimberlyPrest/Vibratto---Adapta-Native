# Changelog — CRM Vibratto

## [0.0.403] — 2026-09-12 — T3.01 pós-conclusão: canal TikTok + correções da home

### Adicionado

- **Canal TikTok** na atribuição granular (pedido da cliente, 12/09): migration 0120 (atribuição direta `field.values = [...]` — padrão provado da 0112; as tentativas com `field.set('values', ...)` nas migrations 0115/0117/0119 NÃO persistiram neste runtime) + opção no formulário de oportunidade (UI). Provas: create com canal=tiktok 200; valor inválido 400 (regressão); base limpa após migration 0121 (3 negócios reais).
- **Home**: card Base de Contatos clicável → /contatos (v0.0.379, mesmo padrão do card Pipeline Comercial v0.0.374).

### Corrigido

- **Causa raiz do 400 em servico=tesouraria**: hook `commercial_contract.js` (T2.01) validava `servico` contra lista SERVICES hardcoded sem tesouraria, em DOIS callbacks (create e update). Fix em ambos (v0.0.378). Aprendizado registrado: AP-2026-09-12-0200.
- Limpezas: migrations 0111 (fixtures T301), 0114 (prova da revalidação), 0121 (prova do TikTok). Base final: 3 negócios reais (Proposta BPO, Proposta CFO, Felicidade Collective).

### Lição técnica nova (candidata AP)

- No JSVM do Skip, `field.set('values', arr)` em select NÃO persiste; a atribuição direta `field.values = arr` persiste. Validado por contraste: 0112 (atribuição direta) funcionou; 0117/0119 (set) aplicaram sem efeito observável.
