# AP-2026-09-13-0040 — JSVM delete em migration + menu global

**Padrão:** 3 falhas recorrentes da T3.02 com causa demonstrada.

1. **Delete de registro em migration**: `$app.delete(rec)` NÃO remove (falha silenciosa em try/catch — 0131–0134 não removeram fixtures). Assinatura correta: `app.delete(rec)` no app da migration (0135 removeu as 2). Complemento da lição AP-2026-09-12-0200 (escopo JSVM).
2. **Menu fechando no mesmo clique**: listener global `document.addEventListener('click', fechar)` fecha menus em qualquer clique, inclusive o do botão que abre. Correção: `ev.stopPropagation()` no botão. Verificado no browser real.
3. **Conflito de rotas no routescan**: `POST /{id}/enviar` conflita com `POST /publico/{token}` (ambos casam com `/publico/enviar`). Padrão: segmento de ação antes do ID (`/enviar/{id}`).

**Evidência:** provas API da T3.02 + verificação browser (menu 4 itens) + contagem 0 fixtures após 0135.
