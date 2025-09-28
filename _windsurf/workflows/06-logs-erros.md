---
description: Modo 6 — Logs & Errors (observabilidade)
auto_execution_mode: 1
---

Modo: OBS

Objetivo:
- Padronizar logs (níveis, correlação, PII-safe)
- Tratar erros de forma consistente (mapeamento para códigos/retornos)

Tarefa:
- Substituir prints/console por logger canônico
- Introduzir handler/exception middleware adequado
- Não alterar contratos públicos

Saída:
- Diffs dos arquivos tocados
- Nota curta de como testar manualmente
