# SPEC-3-008 — Harmonização visual dos cards

**Task:** T3.09 (Fase 3 — leva 9, pedido da CEO 13/09)
**Base:** pedido direto da CEO com prints de referência (cards "Pipeline Comercial", "Base de Contatos", "Contas & Empresas" da home).
**Princípio:** um só padrão visual em todo o CRM — os cards de qualquer tela devem parecer irmãos dos cards de módulo da home.

## 1. Padrão de referência (cards de módulo da home)

- Fundo bege claro `#F7F5F1`, borda `#E5E7EB`, arredondamento `rounded-xl`
- Ícone em quadrado preto `#0A0A0A` (40px, `rounded-lg`) com glifo dourado `#E8C766`
- Título bold (Playfair) preto
- Descrição cinza `#6B7280`
- CTA dourado `#A8862B` com seta "→"
- Hover: borda dourada `#C9A227/60` + sombra

## 2. Escopo

### 2.1 Cards de oportunidade (Opportunities.tsx)
- Ícone preto + glifo dourado no canto do card (por serviço: BPO/Tesouraria/Controladoria/CFO/Outro — glifo lucide correspondente).
- Badge de etapa com fundo bege + borda dourada sutil (hoje é cinza genérico).
- Botões "Editar / Qualificar / Diagnóstico" padronizados: estilo pill com borda `#C9A227/50`, hover dourado; "Mais ⌄" com o mesmo tratamento.
- Valor em destaque tipográfico (bold) — número que decide a leitura.

### 2.2 Cards de automação (Operacional.tsx)
- Mesmo padrão: ícone preto + glifo dourado por tipo de regra (follow-up, SLA, sem ação, parada).
- Contagem em destaque (número grande bold; vermelho quando > 0, cinza quando 0).
- Lista de itens com CTA dourado "Abrir →".

### 2.3 Cards de fila (Operacional.tsx — próximas ações vencidas, filas distintas)
- Mesmo padrão de ícone/título/descrição/CTA.

### 2.4 Cards do Meu dia (MeuDia.tsx)
- Já nasceram no padrão — apenas conferência de consistência (sem alteração esperada).

## 3. Fora do escopo
- Mudança de layout/grade das páginas; novos campos; mudança de tipografia global; cards de formulário público (já seguem a marca).

## 4. Critérios de aceite e provas

| ID | Critério | Prova |
|---|---|---|
| CA-3-025 | Cards de oportunidade seguem o padrão (ícone preto+dourado, badge bege/dourado, botões pill dourados, valor em destaque) | Teste humano: comparação visual com os cards da home |
| CA-3-026 | Cards de automação e fila seguem o padrão (ícone, contagem destacada, CTA dourado) | Teste humano: painel Operacional |
| CA-3-027 | Nenhuma regressão funcional — todos os botões/links continuam operando | Teste humano: abrir editar/qualificar/diagnóstico/menu de uma oportunidade |

## 5. Riscos e cuidados
- Mudança apenas visual: nenhum endpoint, coleção ou regra é alterado.
- Acessibilidade: contraste mantido (texto preto/cinza sobre bege; dourado só em elementos grandes ou com peso semântico).
- Sem alteração de comportamento: hover/estados ativos preservados.
