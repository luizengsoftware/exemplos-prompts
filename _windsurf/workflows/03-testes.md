---
description: Modo 3 — Tests (caracterização antes de mexer)
auto_execution_mode: 1
---

Modo: TESTS

Contexto:
- Framework de testes: {testes}
- Unidade sob teste: {arquivo ou classe/módulo}
- Estado atual: poucos/nenhum teste

Tarefa:
Crie 3–6 testes de caracterização cobrindo:
- Caminho feliz, limites/nulos, erros esperados
- Casos com dados reais/sintéticos minimamente representativos
- Sem over-mock; foque unidade

Saída:
- Arquivo(s) de teste completo(s)
- Instruções de execução ({comando_test})
