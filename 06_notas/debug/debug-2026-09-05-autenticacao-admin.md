# Debug Summary — autenticação administrativa T1.2

- Data: 2026-09-05
- Task: T1.2
- Sintoma: cliente relatou que a senha administrativa não autenticou.
- Reprodução: no preview, o botão oficial `Preencher Demonstração` carregou a fixture administrativa e o envio autenticou com redirecionamento para `/home`.
- Causa raiz confirmada: não foi reproduzida falha da fixture administrativa; o teste manual anterior usou valor diferente ou preenchimento incompleto da credencial.
- Correção de produto: nenhuma. O RBAC fail-closed da versão 0.0.5 foi preservado.
- Evidência: preview CRM_VIBRATTO v0.0.5, redirecionamento observado para `/home`; logs anteriores registram autenticações HTTP 200 e falhas 400 distintas.
- Próximo passo: teste humano usando o botão `Preencher Demonstração`, seguido de validação do operador e bloqueio de `/admin`.
- Privacidade: nenhuma senha ou token registrado neste arquivo.
