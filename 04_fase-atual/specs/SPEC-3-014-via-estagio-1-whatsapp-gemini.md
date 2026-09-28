# SPEC-3-014 — T3.14 V.ia estágio 1: qualificação no WhatsApp (API oficial) + Gemini (Rodada 2)

**Origem:** documento da CEO (23/09/2026, seção 10) + decisões da consultoria de 23/09: V.ia com **WhatsApp API oficial (Cloud API)** e **Gemini 2.5 Flash**, com painel de configuração/teste; integrações de canais (site/Meta/LinkedIn/TikTok — seção 9) FORA desta rodada.
**Gates de ativação:** `via_config.ativo = false` por padrão; ligar somente após `GEMINI_API_KEY`, `WHATSAPP_TOKEN` e `WHATSAPP_APP_SECRET` nos segredos do Skip + número comercial configurado — ativação real reservada à consultora/cliente (padrão da casa).

## Recorte desta rodada

1. **Coleções novas:**
   - `via_config` — configuração editável pela Diretoria sem programação (ativo, phone_number_id, waba_id, verify_token, modelo, prompt_sistema, temperatura, lembrete_intervalo_min, horario_comercial, versao). **A chave Gemini NUNCA no banco** — vive nos segredos do Skip (`$secrets.get('GEMINI_API_KEY')`); a config só marca se está configurada;
   - `via_conversas` — estado da qualificação por número (máquina de estados: nova → qualificando → qualificado | agendado | humano | abandonada | cliente_ativo), respostas, score, temperatura_lead, vínculos lead/negócio/responsável;
   - `via_mensagens` — trilha completa da conversa (append-only, delete bloqueado; entrada/saida; texto/botao/lista/sistema; origem webhook/teste/sistema);
   - `via_testes` — histórico de testes do painel.
2. **Webhook Cloud API:** GET de verificação (hub.mode/hub.verify_token/hub.challenge com o verify_token da config); POST de mensagens com validação de assinatura `X-Hub-Signature-256` (HMAC SHA-256) quando `WHATSAPP_APP_SECRET` estiver configurado; sem secret configurado, aceita e registra aviso (ambiente de teste).
3. **Fluxo (seção 10.1 do documento):** reconhecimento do número — cliente ativo NÃO é qualificado e a conversa segue para o responsável operacional; lead já cadastrado retoma o histórico; somente número novo inicia a qualificação. Boas-vindas com apresentação da V.ia + aviso de privacidade (LGPD). UMA pergunta por vez, preferencialmente com botões, roteiro configurável sem código (máx. 6 perguntas — recomendação da CEO). Classificação quente/morno/frio pelos critérios configurados no menu Qualificação. Qualificado → link para agendar; não qualificado → conteúdo de valor + régua de relacionamento, sem descarte. Lead criado no pipeline com origem WhatsApp, respostas na ficha de qualificação e conversa completa na timeline 360º.
4. **Transbordo e abandono:** pedido de atendimento humano em qualquer momento encaminha e registra pendência; fora do horário comercial, informa horário de retorno e registra pendência para o comercial; abandono → UM único lembrete após intervalo configurável; respostas parciais salvas; lead criado mesmo assim com indicação de qualificação incompleta.
5. **Gemini:** chamada server-side (`x-goog-api-key` dos segredos); sem chave configurada, o teste retorna erro explícito "Configure GEMINI_API_KEY nos segredos do Skip" — nunca resposta inventada; prompt_sistema e temperatura editáveis pela Diretoria.
6. **Painel Configurações › V.ia:** salvar configuração, testar 1 mensagem, simular conversa inteira, histórico de testes, monitoramento de conversas reais, envio admin de teste.
7. **Agendamento (Google Agenda + cópia Outlook):** estrutura pronta (campo/link na qualificação); a integração real de agenda fica para a leva seguinte — recorte honesto registrado.

## Fora do recorte

- Integrações de canais da seção 9 (site/Meta/LinkedIn/TikTok) — spec própria.
- V.ia estágio 2 (rascunho revisado por humano) e estágio 3 — etapas 4/5 do backlog.
- Integração real de agenda (Google Agenda/Outlook) — leva seguinte.

## Critérios de aceite

- **CA-3-049:** webhook verifica o Meta (hub.challenge) e valida assinatura HMAC quando app secret configurado; payload inválido → 400.
- **CA-3-050:** número de cliente ativo não é qualificado e vai ao responsável operacional; lead cadastrado retoma; número novo inicia qualificação.
- **CA-3-051:** qualificação uma pergunta por vez com roteiro configurável sem código; classificação quente/morno/frio conforme critérios configurados.
- **CA-3-052:** transbordo humano em qualquer momento; fora do horário registra pendência; abandono gera 1 lembrete único, preserva respostas parciais e cria lead com qualificação incompleta.
- **CA-3-053:** lead criado no pipeline com origem WhatsApp; respostas na ficha de qualificação; conversa completa na timeline 360º; `via_mensagens` append-only (delete bloqueado).
- **CA-3-054:** painel de configuração/teste operável pela Diretoria sem código; segredos só no servidor; sem chave configurada o teste retorna erro explícito (nunca resposta inventada); histórico de testes e monitoramento de conversas.

## TDD

- **RED:** 403 na verificação com token errado; 400 payload inválido; cliente ativo não qualificado; delete de `via_mensagens` bloqueado; teste sem chave → erro explícito.
- **GREEN:** fluxo completo de número novo até classificação (fixture); retoma de lead cadastrado; transbordo humano; lembrete único (2º bloqueado); registro em pipeline/ficha/timeline.
- **REGRESSÃO:** RBAC (`via_config` na matriz de permissões) e timeline 360º intactos.

## Decisões pendentes da CEO (seção 10.5)

- Faixas de faturamento e critérios de pontuação (o que separa quente/morno/frio).
- Destino do lead frio (conteúdo/produto oferecido — ex.: mentoria ou curso).
- Verificação de disponibilidade do nome/domínio V.ia antes de qualquer divulgação externa.
