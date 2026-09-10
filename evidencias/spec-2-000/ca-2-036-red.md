# Evidência T2.01 — RED (CA-2-036)

- Task: T2.01 — Implementar e provar CA-2-036 (campos comerciais canônicos)
- SPEC: SPEC-2-000 — Remediação dos débitos da Fase 1
- Data: 2026-09-10
- Projeto Skip: CRM_VIBRATTO (id 53851)
- Snapshot analisado: v0.0.84 (031a9d3), migrations 0001–0018 aplicadas

## Lacuna demonstrada (inspeção do schema em vigor)

`schema.json` (gerado em 2026-09-09T19:03:04Z, reflete as migrations aplicadas) mostra que a
coleção `negocios` NÃO possui os 8 campos exigidos pelo contrato canônico da SPEC-1-004
(CA-2-036): origem, tags, responsavel, prioridade, score, servico, status, data_entrada.

## Comportamento atual (falha reproduzível)

- Payload de criação **sem** os 8 campos é aceito — o registro nasce válido, contrariando o
  contrato canônico que exige os campos com validação e auditoria.
- Não há validação server-side para score fora de 0–100 ou status divergente do estágio.
- A auditoria (migration 0010) registra somente `create` e `update`; delete admin-only não
  gera evento append-only.

## Prova por API executada contra o backend em vigor (v0.0.84)

- PATCH `negocios/3j50zuxnwyiw7nw` com score/origem/prioridade → **400** (campos inexistentes).
- POST de negócio sem os 8 campos → aceito pelo sistema (comportamento do snapshot).
- score=150 nunca rejeitado server-side; DELETE sem trilha.

## Resultado

**RED CONFIRMADO.** A lacuna dos 8 campos comerciais, a ausência de validação server-side e a
auditoria incompleta (sem delete) estão demonstradas contra o snapshot v0.0.84.
