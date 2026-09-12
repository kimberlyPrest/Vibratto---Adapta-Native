# Evidência T3.11 — Teste humano executado (a pedido da CEO, 2026-09-13 12:33)

Execução: Deni.Ai no backend interno do preview, com dados REAIS (Felicidade Collective).
Versão testada: v0.0.488 (QA verde).

## Cenário real criado

Ficha operacional da **Felicidade Collective** (empresa real do CRM):
- Serviços: contas_a_pagar, faturamento, conciliacao
- Sistema: Omie (cofre: COFRE-OMIE-FELICIDADE)
- Projeção semanal (segunda), autorização por WhatsApp da Maria Rodrigues, prazo 24h
- Regra fixa (aluguel/folha) vs variável (>R$ 2.000 exige autorização)
- 2 canais (e-mail dedicado + grupo WhatsApp), 1 banco (Itaú, cofre COFRE-ITAU-FELICIDADE), 1 pessoa (Maria — autoriza projeção/faturamento)

## Resultados (23 testes)

| # | Teste | Resultado |
|---|---|---|
| 1 | Criar ficha real com 9 blocos | ✅ 200 |
| 2 | Ler ficha completa (todos os parâmetros) | ✅ todos presentes |
| 3 | Atribuir analista titular | ✅ 200 |
| 4 | Cadastrar 2 canais de entrada | ✅ 200/200 |
| 5 | Cadastrar conta bancária | ✅ 200 |
| 6 | Cadastrar pessoa do cliente | ✅ 200 |
| 7 | **Procedimento gerado (contas a pagar)** | ✅ texto completo e legível com TODOS os parâmetros + aviso de credenciais |
| 8 | Alterar 2 parâmetros | ✅ 200, campos identificados |
| 9 | Versionamento automático | ✅ v1→v6 por serviço, autor/data/campos alterados |
| 10 | Versão anterior preservada | ✅ v1 "segunda-feira" ≠ v2 "segunda e quinta-feira" |
| 11 | **Bloqueio de credencial** | ✅ 400 "Registre apenas o IDENTIFICADOR do item no cofre" |
| 12 | Serviço não contratado (fechamento) | ✅ 400 |
| 13-17 | Operator fora da carteira (ler/procedimento/criar/alterar) | ✅ 403 em todos |
| 18 | Delete bloqueado (histórico) | ✅ 403 |
| 19 | Operator DENTRO da carteira (reserva) | ✅ 200 |
| 20 | Auditoria | ✅ 8 eventos (create/update com campos alterados) |
| 21 | Regressão do zero (negócios/leads/comentários/formulário) | ✅ intactos |
| 22 | Empresa sem ficha | ✅ 404 |
| 23 | Estado final | ✅ ficha real íntegra, operator de teste removido |

## Destaques do procedimento gerado (TESTE 7)

O texto gerado contém: titular/reserva, fora do escopo, regras de contas fixas vs variáveis, quem autoriza e por qual canal, prazo de resposta, destino de comprovantes, sistema + identificador do cofre, contas bancárias com quem aprova no banco, canais de entrada com frequência, e pessoas do cliente com papéis. Exatamente o documento de onboarding que o analista precisa.

## Observação de qualidade (não bloqueia)

PATCH que altera apenas responsável também gera versão nova — comportamento correto (o nome do titular aparece no procedimento), mas gera versões "administrativas". Se ficar ruidoso no dia a dia, filtrar na Leva B.

## Estado final

- Ficha real da Felicidade preservada (titular: Deniane Bezerra, reserva vago)
- 18 versões no histórico (6 PATCHes × 3 serviços — sem duplicatas)
- Nenhum dado de teste residual (operator removido, reserva limpa)
