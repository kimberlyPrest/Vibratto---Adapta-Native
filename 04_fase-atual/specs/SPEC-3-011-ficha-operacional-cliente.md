# SPEC-3-011 — Ficha Operacional do Cliente (Leva A do módulo de operação)

**Task:** T3.11 (Fase 3 — leva 11, aditiva — documento da CEO 12/09)
**Base:** "Ficha Operacional do Cliente — Especificação Dev" v1.0 (capítulos 2 e 3 + 8 + 9). Acesso limitado aos analistas (pedido da CEO).
**Princípio:** um processo único com configurações diferentes por cliente. O procedimento é TEXTO GERADO a partir dos parâmetros — manutenção no cadastro, nunca no documento.

## 1. Objetivo

Registro único por cliente ativo com os 9 blocos de parâmetros operacionais; a partir deles, o sistema gera o procedimento operacional legível por serviço contratado (contas a pagar, faturamento, conciliação, fechamento), versionado a cada alteração. Acesso por permissão: direção/coordenação leitura+escrita; analista leitura dos clientes da carteira; comercial sem acesso.

## 2. Escopo

### 2.1 Coleção `fichas_operacionais` (1 por empresa)
- empresa (relation empresas, único), status_operacional (em_implantacao|ativo|suspenso|encerrado), data_inicio_operacao, responsavel_principal (users), responsavel_reserva (users, obrigatório), servicos_contratados (multi: contas_a_pagar|faturamento|conciliacao|fechamento|tesouraria|controladoria), fora_do_escopo (text), volume_referencia_pagamentos (number), volume_referencia_notas (number).
- Bloco 3 sistema: sistema (omie|nibo|outro), sistema_outro (text), identificacao_empresa_sistema (text), modulos_utilizados (multi), item_cofre_sistema (text — IDENTIFICADOR, nunca credencial).
- Bloco 5 contas a pagar: periodicidade_projecao (semanal|quinzenal|decendial|mensal), dias_referencia (text), janela_coberta, regra_conta_fixa, regra_conta_variavel, autoriza_projecao (text — pessoa do cliente), canal_autorizacao (email|whatsapp|sistema), prazo_resposta_horas (number), antecipacao_pagamento (bool), destino_comprovantes, estrutura_adicional, controle_externo_cliente.
- Bloco 6 faturamento: origem_informacao (multi), dia_envio_relatorio, aprova_relatorio (text), dia_emissao, rotas_emissao (multi: sistema|portal_prefeitura|invoice|nota_debito), regra_rota, destinatarios_nota, cancelar_previsao (bool), destino_notas, prazo_validacao_final, regra_cobranca.
- Bloco 7 conciliação: frequencia (diaria|semanal|outra), responsavel_conciliacao (users), origem_extrato (manual|arquivo|integracao), destino_comprovantes_conc, controle_externo_conc.
- Bloco 8 fechamento: contabilidade_nome, contabilidade_contato, formato_entrega (por_categoria|por_data|outro), canal_entrega (email|pasta|sistema), prazo_entrega, documentos_exigidos (multi), particularidades.
- Regras: create/update admin+coordenação (role admin no recorte atual — coordenação entra na Leva C com perfis); leitura autenticada; delete bloqueado. Nenhuma credencial em campo algum (validação no hook: bloquear valores que pareçam senha/token).

### 2.2 Coleção `ficha_canais` (Bloco 2 — múltiplos por ficha)
- ficha (relation), tipo_canal (email_dedicado|grupo_whatsapp|planilha_nuvem|sistema_cliente), identificacao, frequencia_verificacao (diaria|semanal|mensal|sob_demanda), finalidade (multi: contas_a_pagar|faturamento|autorizacao|documentos), observacao.

### 2.3 Coleção `ficha_bancos` (Bloco 4 — múltiplos por ficha)
- ficha (relation), banco (itau|bradesco|inter|outro), apelido_conta, finalidade (pagamentos|recebimentos|ambos), perfil_acesso (operacional_sem_aprovacao|consulta|outro), quem_aprova_no_banco (text — pessoa do cliente), item_cofre (text), data_ultima_revisao_acesso (date).

### 2.4 Coleção `ficha_pessoas` (Bloco 9 — múltiplos por ficha)
- ficha (relation), contato (relation clientes), papel_operacional (multi: autoriza_projecao|aprova_banco|aprova_faturamento|envia_informacao|apenas_informado), canal_preferencial (email|whatsapp), ativo (bool).

### 2.5 Procedimento gerado (cap. 3)
- `GET /backend/v1/fichas/{empresaId}/procedimento` (auth) — gera texto legível POR SERVIÇO contratado a partir dos parâmetros vigentes; versão calculada de um hash dos parâmetros; sem credencial, sem caminho de pasta de acesso.
- Versionamento: coleção `ficha_versions` (ficha, servico, versao, conteudo, gerado_em, gerado_por, campos_alterados json) — nova versão gravada quando parâmetro muda (hook no update da ficha compara campos e registra autor/data/campo).
- Exportação: o texto gerado é copiável (UI) — PDF fica para leva futura se a CEO pedir.

### 2.6 UI
- Aba "Operação" na Consulta 360º do negócio OU página `/operacao/{empresaId}`: formulário da ficha em 9 blocos (accordion), listas de canais/bancos/pessoas com adicionar/remover, botão "Ver procedimento vigente" por serviço, histórico de versões.
- Acesso: admin vê tudo; operator (analista) vê fichas dos clientes da carteira (responsavel_principal ou reserva = ele) — leitura na Leva A; comercial não vê (menu escondido + rule).
- Aviso permanente: "Nunca registre senhas aqui — use o identificador do cofre."

## 3. Fora do escopo (Leva A)
- Motor de rotinas e exceções (Leva B); visão de coordenação e implantação (Leva C); integração com cofre; exportação PDF; permissão "coordenação" como papel distinto (usa admin no recorte).

## 4. Critérios de aceite e provas

| ID | Critério | Prova |
|---|---|---|
| CA-3-032 | Ficha completa salva com 9 blocos e listas (canais/bancos/pessoas) | RED: sem auth 401; ficha duplicada para mesma empresa 400; GREEN: ficha criada com todos os blocos e 2 canais/2 bancos/2 pessoas |
| CA-3-033 | Procedimento gerado legível por serviço, sem credencial | GREEN: GET procedimento retorna texto com parâmetros do cliente; texto NÃO contém padrão de credencial; RED: empresa sem ficha 404 |
| CA-3-034 | Versionamento automático a cada alteração de parâmetro | GREEN: alterar dias_referencia → nova versão com autor/data/campo alterado; versão anterior preservada |
| CA-3-035 | Acesso limitado: analista vê só a carteira dele; bloqueio de credencial | RED: operator acessa ficha de cliente fora da carteira → 403; campo com valor tipo senha → 400 com mensagem do cofre |

## 5. Riscos e cuidados
- Credencial: validação server-side rejeita valores com padrão de senha/token nos campos item_cofre (que só aceitam identificadores); aviso permanente na UI.
- Dados sensíveis de clientes (LGPD): ficha contém pessoas e contatos do cliente — acesso por permissão, auditoria em create/update.
- Volume: 1 ficha por empresa, listas pequenas — sem risco de performance.
- Decisões pendentes da CEO (cap. 11) NÃO bloqueiam a Leva A: prazos de resposta e volumes são campos preenchíveis depois; cofre é só identificador.
