# SPEC-3-001 — Formulários inteligentes por solução (BPO / CFO as a Service / Consultoria)

**Task:** T3.02 (Fase 3 — leva 2)
**Base:** documento "Onda 3 — Conexão, Qualificação e Conversão" (seções 5, 6, 7, 8 e 9), confirmado pela cliente como complementar em 12/09/2026.
**Princípio central (da cliente):** a proposta não deve nascer apenas da conversa do vendedor — deve nascer dos dados coletados na qualificação, no mapeamento e no diagnóstico.

## 1. Objetivo

O CRM identifica a solução de interesse da oportunidade → gera um link de formulário específico (BPO, CFO ou Consultoria) → o cliente responde sem login → as respostas retornam ao CRM, ficam vinculadas a contato/empresa/oportunidade e atualizam campos estruturados que preparam a proposta — sem copiar e colar.

## 2. Escopo (recorte mínimo completo)

### 2.1 Coleção `formularios` (respostas)
- Campos: id, token (único, imutável), oportunidade (relation negocios), contato (relation), empresa (relation), solucao (select: bpo_financeiro | cfo_as_a_service | consultoria), status (select: gerado | enviado | respondido | expirado), respostas (json), resumo (text), gerado_por (relation users), enviado_em (date), respondido_em (date), created/updated.
- Regras de API: criação/leitura apenas server-side (hook); leitura pública SOMENTE via token (rota pública dedicada); sem listagem pública; delete bloqueado (append-only); auditoria em toda mudança de status.

### 2.2 Geração do formulário na oportunidade
- Botão "Formulário" no card da oportunidade → escolha da solução (pré-preenchida com o `servico` da oportunidade) → cria registro com token → exibe link público copiável.
- Link público: `/formulario/<token>` (rota frontend pública, sem autenticação).

### 2.3 Formulários públicos por solução (frontend)
- BPO Financeiro (15 campos do doc Onda 3 §6): identificação responsável/empresa, CNPJ, regime tributário, pagamentos/mês, recebimentos/mês, funcionários, sócios e modelo societário, principais fornecedores, bancos, câmbio (sim/não), tipos de receita, desafios do financeiro, desafios do negócio, endividamento ativo, sonho/objetivo 12 meses, origem do contato + consentimento LGPD (obrigatório).
- CFO as a Service (9 campos §7): como decisões são tomadas, orçamento anual/mensal (sim/não), fluxo de caixa projetado (sim/não), indicadores acompanhados, DRE gerencial (sim/não), frequência de análise, decisões travadas por falta de informação, objetivos 6/12/24 meses, nível de participação esperado do CFO.
- Consultoria (8 campos §8): problema, impacto atual, o que já foi tentado, resultado esperado, prazo, envolvidos, critério de sucesso, restrições.
- UX: campos obrigatórios marcados; texto longo em textarea; consentimento LGPD obrigatório nos 3 formulários; confirmação de envio com mensagem clara; página de token inválido/já respondido.

### 2.4 Atualização automática da oportunidade (doc §9)
Ao responder, o hook server-side grava em `negocios` os campos estruturados (json `dados_formulario` + campos diretos quando existirem):
- dados cadastrais/perfil (CNPJ, regime, funcionários, sócios, cidade quando informada);
- volume operacional (pagamentos/mês, recebimentos/mês);
- complexidade (bancos, câmbio, tipos de receita);
- dores (desafios financeiro/negócio OU problema/impacto);
- objetivos (sonho 12m OU objetivos 6/12/24m OU resultado esperado);
- urgência (prazo informado);
- insumos de precificação/escopo (volumes, endividamento, nível de senioridade implícito).
- A oportunidade recebe `formulario_status = respondido` e o resumo gerado.
- NUNCA sobrescreve campos comerciais gerenciados pelo CRM (valor, estágio, responsável, atribuição) — só campos de contexto.

### 2.5 Resumo para análise comercial
- Hook gera `resumo` (texto estruturado, sem IA nesta task — IA é Fase 4/5) com: perfil, volume, dores, objetivos, urgência. Exibido na oportunidade (bloco "Formulário").

### 2.6 Auditoria e LGPD
- Toda transição de status do formulário gera registro de auditoria com ator/sistema, data e snapshot mínimo.
- Consentimento LGPD registrado com data e versão do texto.
- Dados sensíveis (CNPJ, volumes) trafegam apenas no backend; link público exige token de 32+ chars aleatório.

## 3. Fora do escopo desta task
- Envio automático por WhatsApp/e-mail (integrações — próximas levás); o botão copia o link para o time enviar manualmente.
- Ficha de preparação da proposta (T3.02b, SPEC própria).
- IA (resumo inteligente, classificação) — Fase 4/5.
- Expiração automática de token (registro manual de status expirado apenas).

## 4. Critérios de aceite e provas (TDD)

| ID | Critério | Prova |
|---|---|---|
| CA-3-002 | Formulário público por solução gerado na oportunidade, respostas gravadas vinculadas a contato/empresa/oportunidade | RED: POST público sem token 403/404; GREEN: gerar token → responder via rota pública → registro `formularios` com vínculos completos; token inválido → página de erro; reenvio do mesmo token → bloqueado |
| CA-3-003 | Oportunidade atualizada automaticamente com campos estruturados após resposta | RED: oportunidade sem dados_formulario; GREEN: após resposta, json preenchido + formulario_status=respondido + resumo gerado; campos comerciais intocados (regressão) |
| CA-3-004 | Consentimento LGPD obrigatório e auditado | RED: resposta sem consentimento 400; GREEN: com consentimento, registro com data/versão; auditoria contém a transição |
| CA-3-005 | Delete bloqueado e auditoria de status | RED: DELETE 403; GREEN: transições gerado→enviado→respondido auditadas com ator e data |

## 5. Riscos e cuidados
- Formulário público = superfície de ataque: token aleatório, sem listagem, rate limit básico na rota pública, sem revelar existência de outros tokens.
- Campos de CNPJ: validar formato; nunca logar conteúdo de respostas.
- Não quebrar o guard de negócio ativo (T2.18) ao atualizar a oportunidade via hook interno (save de sistema).
- Mobile-first: o cliente responde pelo celular via link do WhatsApp.

## 6. Evidência esperada
- evidencias/spec-3-001/ca-3-002.md … ca-3-005.md (RED/GREEN por API)
- Caso real: formulário BPO gerado para a oportunidade Proposta BPO, respondido com dados de teste da própria Vibratto, resumo visível na oportunidade.
- Teste humano: Deniane gera o link, responde no celular e vê o resumo na oportunidade.
