# AP-2026-09-12-0230 — Canais de atribuição: técnica da atribuição direta confirmada em 3ª aplicação

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: T3.01 pós-conclusão (migrations 0128/0129, v0.0.404–0.0.407)
- Sinal: adição de canais (`comunidade`, `spotify`, `podcast`) à lista do select `canal` persistiu somente com atribuição direta `field.values = [...]`; o padrão já havia sido confirmado para `servico` (0112) e canais (0125). Terceira aplicação consecutiva sem falha.
- Evidência: QA verde v0.0.404/0128 e v0.0.405/0129; provas por API v0.0.407 (PATCH canal=comunidade/spotify/podcast 200; inválido 400); changelog 0.0.407.
- Regra reutilizável: em migrations JSVM deste runtime, alterar options de select SEMPRE com `field.values = array` (atribuição direta); nunca `field.set('values', ...)` nem remove+add.
- Quando aplicar: qualquer migração que adicione/remova opções de select (canal, serviço, motivo, status).
- Quando não aplicar: campos não-select (text/relation) seguem o padrão normal de migration de schema.
- Confiança: alta — três aplicações independentes com mesmo resultado verificável.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto.
