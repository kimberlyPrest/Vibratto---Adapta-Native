# SPEC-3-013 — T3.13 RBAC: perfis, permissões e gestão de usuários (Rodada 1)

**Origem:** documento da CEO "CRM Vibratto — Especificação: usuários, permissões, integrações e qualificação via WhatsApp" (23/09/2026, seções 1–8), aprovado pela consultora para implementação em rodadas — RBAC primeiro; integrações de canais (seção 9) ficam para spec própria.
**Princípio inegociável (CEO, seção 5):** validação no SERVIDOR e no banco — nunca apenas ocultação de itens de menu. Acesso direto por URL a página não permitida retorna mensagem de falta de permissão, sem exibir nenhum dado.

## Recorte desta rodada

1. **`users` ganha `perfil`** (diretoria | coordenacao | analista | comercial | conteudo) e `tokens_invalidos_apos` (revogação imediata de sessão).
2. **Coleções novas:**
   - `grupos_permissao` — matriz editável pela Diretoria (item → edita | leitura | negado), seed dos 5 grupos padrão conforme a seção 4 do documento;
   - `permissoes_excecao` — exceção individual por usuário+item (sobrescreve o grupo), com motivo;
   - `usuarios_clientes` — clientes atribuídos ao analista (base da visibilidade "somente os clientes atribuídos").
3. **Resolução server-side (ordem):** role=admin → edita (origem role_admin); exceção individual sobrescreve; grupo do perfil; sem grupo/exceção → negado (fail-closed). Endpoints: `GET /backend/v1/rbac/permissao?item=` e `GET /backend/v1/rbac/minhas` (matriz completa com origens).
4. **Gestão de usuários exclusiva da Diretoria:** listar, convidar/criar (e-mail + perfil + clientes atribuídos; senha gerada server-side quando não informada, retornada UMA vez), alterar perfil/atribuições, desativar (active=false + revogação imediata, sem excluir histórico), reativar, reset de senha. Tudo auditado (quem, quando, estado anterior/posterior).
5. **Frontend:** página Configurações › Usuários e permissões (abas usuários/grupos/exceções). Menu lateral, cards da página inicial, busca global, notificações, links em texto e exportações filtrados pela matriz. Página inicial por perfil. Valores comerciais (proposta, desconto, volume do funil) ocultos dos perfis operacionais.
6. **Migração idempotente** com backfill de perfil nos usuários existentes (admin→diretoria, operator→coordenacao, social_media→conteudo).

## Fora do recorte

- Integrações site/Meta/LinkedIn/TikTok (seção 9 do documento) — spec própria futura.
- Backup com teste de restauração (item 8 do backlog) — task própria.
- Passagem comercial→operação com ocultação do histórico de negociação (seção 5, último parágrafo) — evolui a implantação existente em task própria.
- Decisões em aberto da CEO (seção 8) permanecem como estão até decisão: implantações para o Comercial; painel de direção para a Coordenação; natureza do Kanban.

## Critérios de aceite

- **CA-3-043:** permissão resolvida server-side (endpoint responde `concedido` + `origem`); URL direta a página "Não" bloqueada sem exibir dado.
- **CA-3-044:** matriz dos 5 perfis conforme seção 4 (seed); exceção individual sobrescreve o grupo; item sem regra → negado.
- **CA-3-045:** gestão de usuários exclusiva da Diretoria (403 para os demais); criar/alterar perfil/atribuir clientes sem código.
- **CA-3-046:** desativar usuário com sessão aberta derruba a sessão no próximo refresh (401), sem excluir histórico.
- **CA-3-047:** busca global, notificações, cards da home e exportações respeitam a matriz; valores comerciais não aparecem para perfis operacionais.
- **CA-3-048:** criação/alteração/desativação de usuário gera evento de auditoria com ator, data e estados anterior/posterior.

## TDD

- **RED:** 401 sem auth; 403 não-admin na gestão de usuários; URL direta bloqueada; operacional consultando valor comercial → negado; item inexistente → 400.
- **GREEN:** matriz seed correta para os 5 perfis; exceção sobrescreve grupo; revogação derruba sessão aberta; auditoria registra os 3 eventos; senha gerada retornada 1x.
- **REGRESSÃO:** fluxos das Fases 1–2 (kanban, qualificação, proposta, exportação) intactos para a Diretoria.

## Decisões pendentes da CEO (não bloqueiam o recorte)

- Implantações para o Comercial (leitura ajuda a vender com prazos realistas, mas expõe a fila da operação) — seção 8.
- Painel de direção para a Coordenação (hoje exclusivo da Diretoria) — seção 8.
- Natureza do Kanban (pipeline comercial vs. quadro de tarefas operacionais) — seção 8.
