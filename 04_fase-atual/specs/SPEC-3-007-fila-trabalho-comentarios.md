# SPEC-3-007 — Fila de trabalho pessoal + comentários/menções

**Task:** T3.08 (Fase 3 — leva 8)
**Base:** backlog Etapa 3, itens 4 e 5 ("Fila de trabalho pessoal — painel lateral com registros atribuídos e pendências do dia"; "Comentário no registro + menção direta + notificação interna central — comentário grava na timeline").
**Princípio:** cada pessoa abre o CRM e vê o QUE É DELA hoje — sem caçar em quatro telas. A colaboração fica dentro do registro, com trilha.

## 1. Objetivo

Painel "Meu dia" com tudo que está atribuído ao usuário logado (tarefas abertas, oportunidades com próxima ação vencida, comentários onde ele foi mencionado) + comentários estruturados em negócio/tarefa com menção @nome e notificação interna central (sino no header). Comentário grava evento na timeline 360º.

## 2. Escopo

### 2.1 Coleção `comentarios` (append-only)
- negocio (relation, obrigatório), tarefa (relation, opcional), autor (relation users, obrigatório), texto (text 1–2000, obrigatório), mencoes (json — lista de user ids extraídos server-side), created (autodate).
- Regras: create autenticado; update/delete bloqueados (append-only, padrão das interações).
- Menção: padrão `@Nome` no texto; o hook resolve nomes de usuários ativos e grava ids em `mencoes`. Menção a usuário inexistente não bloqueia o comentário (fica como texto).

### 2.2 Coleção `notificacoes` (append-only, por usuário)
- usuario (relation users, obrigatório), tipo (select: mencao | tarefa_atribuida | comentario), origem (relation negocios, opcional), origem_tarefa (relation tarefas, opcional), comentario (relation comentarios, opcional), lida (bool, padrão false), lida_em (date), created (autodate).
- Regras: create SOMENTE server-side (hooks); leitura apenas do próprio usuário (list/view rule `usuario = @request.auth.id`); update limitado a marcar lida (hook valida: só `lida`/`lida_em` podem mudar); delete bloqueado.
- Geração: (a) comentário com menção → notificação `mencao` para cada mencionado (não notifica o autor); (b) tarefa criada → notificação `tarefa_atribuida` para o responsável (se ≠ autor).

### 2.3 Endpoints (hook `fila_trabalho.js`)
- `GET /backend/v1/meu-dia` (auth) — consolida para o usuário logado: tarefas abertas atribuídas a ele (com prazo e atraso), oportunidades ativas com próxima ação vencida onde ele é responsável, comentários recentes nos negócios que ele responde (últimos 7 dias), contagem de notificações não lidas. Somente leitura; falha de fonte vira aviso (padrão timeline).
- `GET /backend/v1/notificacoes` (auth) — lista as notificações do usuário (filtro lida/não lida, limite 50).
- `POST /backend/v1/notificacoes/{id}/lida` (auth, dono apenas) — marca lida com timestamp.
- `GET /backend/v1/negocios/{id}/comentarios` (auth) — lista comentários do negócio (com nome do autor).
- `POST /backend/v1/negocios/{id}/comentarios` (auth) — cria comentário (texto 1–2000), extrai menções server-side, grava notificações, registra evento na auditoria.

### 2.4 UI
- **Sino no header (Home.tsx)**: badge com contagem de não lidas; dropdown com as últimas notificações (marcar lida ao clicar, link para o negócio/tarefa de origem).
- **Página `/meu-dia`**: painel pessoal com 3 seções — Minhas tarefas abertas (com botão concluir direto), Minhas ações vencidas (link para a oportunidade), Menções recentes (link para o comentário). Card de módulo na home seguindo o padrão visual (ícone preto + glifo dourado, CTA dourado) — primeiro card no NOVO padrão harmonizado.
- **Comentários na oportunidade**: nova entrada "Comentários" no menu Mais ⌄; modal com lista cronológica + campo de texto com dica de menção `@`; menções viram chip destacado.

### 2.5 LGPD e auditoria
- Notificação só é visível ao dono (rule server-side). Comentário é interno ao time (não expõe dados de cliente além do que o time já vê). Auditoria em todo create de comentário (snapshot mínimo).

## 3. Fora do escopo
- Notificação por e-mail/WhatsApp (fase futura); comentário em contato/empresa (só negócio e tarefa); edição de comentário; menção a grupo/equipe; painel por papel (task separada).

## 4. Critérios de aceite e provas

| ID | Critério | Prova |
|---|---|---|
| CA-3-021 | Fila pessoal consolida tarefas abertas + ações vencidas + menções do usuário logado, somente leitura | RED: sem auth 401; GREEN: fixture com tarefa atribuída + ação vencida → meu-dia retorna ambos; usuário B não vê dados de A |
| CA-3-022 | Comentário criado com menção gera notificação para o mencionado e evento na timeline | RED: texto vazio 400, sem auth 401; GREEN: comentário com @Nome → notificacao criada para o mencionado (não para o autor), comentário aparece no endpoint de timeline |
| CA-3-023 | Notificações são privadas por usuário e marcar-lida é auditável | RED: usuário B lê notificação de A → 403/404; marcar lida de outro → 403; GREEN: dono marca lida → lida_em gravado; badge conta só não lidas |
| CA-3-024 | Tarefa atribuída gera notificação para o responsável | GREEN: tarefa criada para usuário B → notificacao tarefa_atribuida para B; RED: create direto em notificacoes → 403 (só server-side) |

## 5. Riscos e cuidados
- `users.list` hoje é `id = @request.auth.id` — o hook resolve nomes server-side (padrão timeline); a UI NÃO depende de listar usuários para menção (autocomplete usa endpoint próprio ou digita @Nome livre).
- Volume: notificações crescem — leitura limitada a 50 + retenção futura no padrão D5 (não neste recorte).
- Menção com nome duplicado: notifica todos os ativos com o mesmo nome (caso raro, documentado).
- Timeline: comentários entram como nova fonte no endpoint existente (T3.04) — alteração aditiva, sem quebrar as 9 fontes atuais.
