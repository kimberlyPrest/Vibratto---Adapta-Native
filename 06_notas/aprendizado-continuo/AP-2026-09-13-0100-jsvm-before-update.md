# AP-2026-09-13-0100 — JSVM: before não confiável no update

**Padrão:** validação de update que compara "antes vs. depois" NÃO funciona em hooks JSVM do runtime Skip (testado em 7 versões: request hook reflete payload parcial; model hook idem).

**Solução provada:** validação de update via **API rule da coleção** (migration): `@request.body.campo != ''` — o PocketBase avalia contra o payload real. Complemento no hook para comprimento (mín. 10 chars).

**Gramática de regras PB (armadilhas):** sem ternário `?`; sem `String().length`; sem `length()` (função inexistente) — só comparações `=`, `!=`, `~`, `&&`, `||`.

**Comportamento:** PATCH sem o campo exigido pela rule retorna **404** (não 400) — tratar no frontend.

**Evidência:** provas API da T3.02b (R5 bloqueado por rule; R3 400 por hook no create).