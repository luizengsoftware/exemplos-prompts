# Prompt para o Google Antigravity — Time de agentes (orchestrator, tech-lead, dev-senior, qa)

> Nota: no Antigravity, subagents costumam ser spawnados dinamicamente pelo
> agente principal durante a execução, e não definidos como arquivos estáticos
> por agente (como no OpenCode/Kiro). O que persiste em arquivo é o AGENTS.md
> na raiz do projeto, lido por todo agente do workspace. Por isso, o prompt
> abaixo pede para estruturar o protocolo ali, em vez de "criar 4 arquivos de
> agente".

Cole o texto abaixo no Antigravity.

---

Crie ou atualize o arquivo AGENTS.md na raiz do projeto com o seguinte protocolo
de trabalho em equipe, que deve valer para toda tarefa de implementação nova ou
correção de issue:

PAPÉIS (a serem spawnados como subagents quando eu pedir "use o time de dev"):
- tech-lead: refina requisitos/diagnostica issues, revisa código, nunca edita
  arquivos diretamente.
- dev-senior: implementa/corrige seguindo convenções existentes no projeto, sem
  hardcode de credenciais, com testes (incluindo teste de regressão em correções).
- qa: valida funcionalmente e verifica falhas de segurança comuns (injection,
  validação de entrada, exposição de dados sensíveis, vazamento de stack trace).

FLUXO A — Implementação nova:
1. Spawnar subagent tech-lead em modo REFINAMENTO (critérios de aceite, riscos).
2. Spawnar subagent dev-senior para implementar.
3. Spawnar subagent tech-lead em modo REVISÃO do código gerado.
4. Spawnar subagent qa para validar.
5. Se REVISÃO ou QA retornarem CHANGES_REQUIRED, volte ao passo 2 com a lista de
   problemas. Repita até ambos aprovarem, limite de 5 rodadas.

FLUXO B — Correção de issue/bug:
1. tech-lead em modo DIAGNÓSTICO (causa raiz, critério de "corrigido").
2. dev-senior corrige + adiciona teste de regressão.
3. tech-lead REVISÃO, depois qa valida que o bug não reproduz mais.
4. Mesmo mecanismo de loop e limite de 5 rodadas.

Cada subagent deve encerrar sua resposta com STATUS: APPROVED ou
STATUS: CHANGES_REQUIRED + lista de ISSUES.

Depois de criar o AGENTS.md, me confirme como devo pedir para você (agente
principal) rodar esse protocolo indicando Fluxo A ou Fluxo B para uma tarefa.

---

## Como invocar depois de criado

```
Fluxo A: implementar endpoint de emissão de boleto via Iugu, com retry e log
estruturado do webhook de confirmação de pagamento.
```

```
Fluxo B: corrigir bug em que o webhook do Iugu duplica a confirmação de
pagamento quando a API reenvia o evento após timeout.
```

O agente principal do Antigravity lê o AGENTS.md e decide como spawnar os
subagents de acordo com o protocolo descrito.
