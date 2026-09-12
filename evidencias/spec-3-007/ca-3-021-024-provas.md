# Evidência T3.08 — CA-3-021/022/023/024 (RED/GREEN por API)

Data: 2026-09-13 · Versões: v0.0.463–0.0.469 (QA verde) · Execução: provas por API no backend interno do preview.

## RED (rejeições corretas)

| Prova | Chamada | Resultado |
|---|---|---|
| R1 meu-dia sem auth | GET /meu-dia sem token | 401 |
| R2 comentário vazio | POST comentarios texto="  " | 400 |
| R3 create direto em notificacoes | POST /api/collections/notificacoes (createRule null) | 403 |
| R4 marcar lida de outro usuário | operator marca notificação da Deniane | 403 |
| R5 re-marcar lida | 2ª marcação da mesma notificação | 400 |

## GREEN (comportamento correto)

| Prova | Chamada | Resultado |
|---|---|---|
| G1 comentário com menção | POST comentarios "@Deniane Bezerra..." (operator) | 200 `{mencoes:1, notificados:1}` |
| G2 notificação criada | GET /notificacoes (Deniane) | 1 mencao, lida:false, negocio_titulo correto |
| G3 meu-dia reflete | GET /meu-dia (Deniane) | mencoes_recentes:1, nao_lidas:1 |
| G4 timeline 10ª fonte | GET /negocios/{id}/timeline | evento tipo "comentario" presente |
| G5 tarefa atribuída notifica | POST tarefas (responsavel=operator) | notificação tarefa_atribuida na lista do operator |
| G6 marcar lida | POST /notificacoes/{id}/lida (dono) | 200 com lida_em gravado |
| G7 privacidade da fila | GET /meu-dia (operator) | só tarefas/notificações dele (6 tarefas de prova, 0 da Deniane) |

## Fix provado no caminho

- v0.0.467: onRecordCreate('tarefas') pré-save NÃO criava a notificação (3 tentativas, 0 eventos de debug em auditoria) → substituído por onRecordAfterCreateSuccess → G5 passou.

## Limpeza

Migrations 0156/0157 (SQL direto) — base final: 0 tarefas/comentários/notificações/usuários de prova; 3 negócios e 3 leads_entrada reais intactos.
