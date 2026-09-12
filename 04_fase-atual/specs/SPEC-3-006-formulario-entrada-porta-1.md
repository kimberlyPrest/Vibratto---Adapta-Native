# SPEC-3-006 — Porta 1: formulário público de entrada

**Task:** T3.07 (Fase 3 — leva 7)
**Base:** documento "Requisitos complementares da etapa 3" (§ formulário de entrada e roteamento) + decisões da CEO (12/09): modelo DUAS PORTAS confirmado; sonho 12 meses sobe para a Porta 1; endividamento desce para a Porta 2; 3 campos novos (CNPJs do grupo, ERP, quem cuida do financeiro).
**Princípio:** capturar o lead em 90–120s no celular, rotear por sintoma e pontuar server-side — a conversa começa com dado, não com palpite.

## 1. Objetivo

Qualquer pessoa acessa `/entrada` sem login, responde em ~2 minutos e o CRM registra o lead com temperatura calculada, origem rastreada e LGPD auditável. Lead quente entra na fila para contato; o vínculo com oportunidade é decisão humana (botão "Vincular").

## 2. Escopo

### 2.1 Coleção `leads_entrada` (append-only)
- Identificação: nome, e-mail, WhatsApp, eh_decisor, CNPJ, empresa_razao (enriquecimento).
- Qualificação: faturamento_faixa (6 faixas com "não sei informar"), qtd_cnpjs, colaboradores, regime_tributario (com "não sei informar"), erp_atual, quem_cuida_financeiro.
- Sintoma: dor_principal (6 opções estruturadas), dores_secundarias, relato, urgencia, sonho_12m.
- Captura: score (number), temperatura (quente|morno|frio), utm (json), origem_declarada, ip, consentimento_lgpd + versão + data, optin_marketing, negocio (relation), status (novo|vinculado), trilha (json).
- Regras: create/update/delete SOMENTE server-side (null); leitura autenticada.

### 2.2 Endpoints
- `GET /backend/v1/entrada/publico` — versão do texto LGPD (público).
- `POST /backend/v1/entrada/publico` — envio público: honeypot, tempo mínimo 20s, validações (nome, e-mail, WhatsApp ≥10 dígitos, CNPJ 14 dígitos se informado, dor obrigatória), consentimento obrigatório, rate limit por IP/hora (config, padrão 3, fail-open com log se IP indetectável), score 0–92 server-side, dedup por e-mail.
- `GET /backend/v1/entrada/leads?temperatura=` — lista interna (auth).
- `POST /backend/v1/entrada/leads/{id}/vincular` — cria contato (dedup por e-mail) + oportunidade saudável (primeira etapa ativa, responsável = ator, próxima ação +7 dias, canal do UTM, entrada_origem=formulario_entrada); re-vinculação bloqueada.

### 2.3 Score (0–92, server-side)
decisor +15; faturamento 500k–2m +15, 2m–10m +20, >10m +20, 100k–500k +8; 2+ CNPJs +8; 5+ colaboradores +7; regime conhecido +5; ERP informado +5; dor de caixa/crescimento +12 (outras +6); urgência imediata +15 / trimestre +10 / ano +5; sonho ≥20 chars +5. Temperatura: quente ≥60, morno 35–59, frio <35.

### 2.4 UI pública `/entrada`
Mobile-first, 3 blocos (A identificação, B qualificação, C sintoma), CNPJ com máscara e enriquecimento BrasilAPI INFORMATIVO (falha não bloqueia, mensagem explícita), UTM capturado da URL, honeypot invisível, LGPD duplo, tela de confirmação.

### 2.5 LGPD e auditoria
Consentimento obrigatório com versão (LGPD-V1-2026-09) e data; opt-in de marketing separado e opcional; auditoria em todo envio/vínculo com snapshot mínimo (sem conteúdo do relato); IP armazenado para rate limit.

## 3. Fora do escopo
- Notificação de lead quente (D6 pendente da CEO); agenda na tela final (D8); pré-preenchimento/salvar-e-retomar da Porta 2; formulários das outras soluções; e-mail de confirmação automático.

## 4. Critérios de aceite e provas

| ID | Critério | Prova |
|---|---|---|
| CA-3-018 | Entrada pública capturada com score/temperatura server-side e dedup por e-mail | RED: dor inválida 400, campos ausentes 400; GREEN: envio válido 200 com score/temperatura corretos; dedup marca `vinculado` + evento na oportunidade (prova humana no teste) |
| CA-3-019 | LGPD obrigatório + rate limit + honeypot + tempo mínimo | RED: sem consentimento 400; tempo <20s 400; GREEN: 4º envio do mesmo IP em 1h → 429; honeypot → 200 silencioso sem registro |
| CA-3-020 | Vincular cria oportunidade saudável sem tocar campos comerciais | RED: sem auth 401, re-vincular 400; GREEN: contato + oportunidade criados (etapa ativa, responsável, próxima ação futura, canal do UTM) |

## 5. Riscos e cuidados
- Superfície pública: token não se aplica (formulário aberto), proteção por honeypot + tempo + rate limit; sem listagem pública; erro não revela existência de dados.
- IP atrás de proxy: fail-open documentado (não bloqueia visitantes legítimos); log registra ocorrência.
- Dedup conservador: só vincula a negócio aberto <90 dias do MESMO e-mail; caso dúvida, o lead fica `novo` para decisão humana.
