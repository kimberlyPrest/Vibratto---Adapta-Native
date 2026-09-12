# Evidência T3.11 — CA-3-032/033/034/035 (RED/GREEN por API)

Data: 2026-09-13 · Versões: v0.0.484–0.0.487 (QA verde) · Execução: provas por API no backend interno do preview.

## RED (rejeições corretas)

| Prova | Chamada | Resultado |
|---|---|---|
| R1 sem auth | GET /ficha-operacional/{empresaId} sem token | 401 |
| R2 empresa sem ficha | GET para empresa sem ficha | 404 |
| R3 credencial em campo | POST com item_cofre_sistema="senha: MinhaSenh@123" | 400 com mensagem do cofre |
| R4 acesso fora da carteira | operator lê ficha onde não é responsável | 403 |
| R5 operator cria ficha | POST /ficha-operacional (operator) | 403 |
| R6 ficha duplicada | POST para empresa já com ficha | 400 |

## GREEN (comportamento correto)

| Prova | Chamada | Resultado |
|---|---|---|
| G1 ficha completa | POST com 9 blocos + status/serviços | 200, ficha criada |
| G2 leitura completa | GET /ficha-operacional/{empresaId} | todos os parâmetros + nomes de responsáveis resolvidos |
| G3 procedimento gerado | GET /procedimento?servico=contas_a_pagar | texto legível com parâmetros do cliente + aviso de credenciais (cofre) |
| G4 versionamento | PATCH altera dias_referencia + prazo_resposta | 2 versões geradas (contas_a_pagar v1 + faturamento v1) com autor/data/campos alterados |
| G5 acesso por carteira | operator como responsavel_reserva | 200 (dentro da carteira) |

## Regra de ouro provada

- Padrões bloqueados: senha:, password:, token:, chave de acesso, api_key, Bearer <token>, base64 longo.
- Campos item_cofre aceitam apenas identificadores (ex.: COFRE-OMIE-001).

## Limpeza

Migration 0161 — base final: 0 fichas/versões/listas de prova; 0 usuários de prova; 3 negócios reais intactos.
