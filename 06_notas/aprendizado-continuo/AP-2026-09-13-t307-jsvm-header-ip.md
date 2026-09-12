# AP-2026-09-13-t307 — Leitura de header HTTP no JSVM (request hook)

**Padrão:** em request hooks do JSVM (goja), headers HTTP NÃO se leem com `e.request.getHeader(...)` (não existe) nem com `e.request.url.query()` (isso é query string). A API correta é `e.request.header.get('Nome-Do-Header')`.

**Caso real (T3.07):** o rate limit do formulário de entrada dependia do IP do remetente. `e.realIp()` retornou vazio atrás do proxy do preview; `e.request.getHeader('X-Forwarded-For')` falhou silenciosamente no try/catch (2 iterações de prova por API até isolar a causa). Fix: `e.request.header.get('X-Forwarded-For')` com fallback `Cf-Connecting-Ip`, provado por 429 no 4º envio.

**Regra derivada:** em hooks JSVM, sempre provar por API qualquer acesso a metadados da requisição (header, query, IP) antes de depender dele em regra de segurança; try/catch vazio esconde o erro e a regra vira fail-open sem ninguém perceber.
