# Vibratto CRM — Código-fonte

Cópia de referência do código-fonte do **CRM_VIBRATTO** (projeto Skip id 53851), exportada em 2026-09-10 na versão **0.0.84** (Fase 1 completa — 24/24 tasks).

## Fonte da verdade

O ambiente executável vive no **Skip Cloud** (PocketBase v0.36 + React/Vite). Esta pasta é uma cópia de leitura para auditoria — alterações devem ser feitas no Skip e re-sincronizadas aqui.

## Estrutura

- `pocketbase/migrations/` — migrations 0001–0018 (coleções, seed, campos, fixtures)
- `pocketbase/hooks/` — hooks JS (auditoria, regras de resultado, migração de etapas, histórico de permanência, filas operacionais, proteção de aceite, fixtures)
- `src/pages/` — telas React (login, home, contatos, oportunidades, kanban, etapas, busca/exportação, painel operacional)
- `src/contexts/`, `src/hooks/`, `src/lib/` — infraestrutura de auth, PocketBase client e utilitários
- `src/lib/pocketbase/schema.json` — snapshot do schema das coleções com regras de acesso

## Credenciais

**Não há credenciais neste repositório.** As senhas dos usuários de teste (admin e operator) são provisionadas por migrations no ambiente Skip e devem ser trocadas no primeiro acesso. O arquivo `.env` é ignorado pelo git.

## Versão

- Skip: CRM_VIBRATTO, id 53851, v0.0.84
- Preview: https://tela-de-login-crm-a400a--preview.goskip.app
- Produção: não publicada
