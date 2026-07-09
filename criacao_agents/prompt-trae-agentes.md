# Prompt para o Trae — Time de agentes (orchestrator, tech-lead, dev-senior, qa)

> Nota: no Trae, Agents costumam ser criados pela própria interface (painel de
> criação de Agent), e o protocolo compartilhado de comportamento fica na pasta
> .trae/rules/ (ou no AGENTS.md, compatível com o mesmo padrão usado por outras
> ferramentas). Não há confirmação de uma pasta .trae/agents/*.md com o mesmo
> formato de front-matter do OpenCode/Kiro — por isso o prompt abaixo pede para
> o próprio Trae criar os Agents pela função nativa dele.

Cole o texto abaixo no chat do Trae.

---

Preciso que você (usando sua função nativa de criação de Agents) crie 4 agents
chamados orchestrator, tech-lead, dev-senior e qa, e que registre o protocolo
de trabalho compartilhado entre eles em .trae/rules/.

Regra compartilhada (.trae/rules/team-workflow.md):
- Time trabalha em loop até entregar sem falhas de segurança ou bugs.
- Dois fluxos possíveis: Fluxo A (implementação nova) e Fluxo B (correção de issue).
- Fluxo A: orchestrator aciona tech-lead (REFINAMENTO) → dev-senior (implementa)
  → tech-lead (REVISÃO) → qa (valida). Se CHANGES_REQUIRED, volta ao dev-senior.
- Fluxo B: orchestrator aciona tech-lead (DIAGNÓSTICO da issue) → dev-senior
  (corrige + teste de regressão) → tech-lead (REVISÃO) → qa (confirma que o bug
  não reproduz mais). Mesmo mecanismo de loop.
- Limite de 5 rodadas de correção; se estourar, reportar o pendente ao invés de
  continuar.
- Cada agent termina a resposta com STATUS: APPROVED ou STATUS: CHANGES_REQUIRED
  + ISSUES.

Papel de cada Agent:
- orchestrator: coordena o fluxo, aciona os demais agents na ordem certa, repassa
  o contexto (requisito ou issue) a cada chamada.
- tech-lead: sem permissão de edição de arquivos; refina/diagnostica e revisa.
- dev-senior: implementa/corrige com acesso total a arquivos e terminal.
- qa: sem permissão de edição; roda testes e verifica segurança.

Depois de criar, me diga como devo @mencionar o orchestrator para rodar o Fluxo A
ou Fluxo B em uma tarefa específica.

---

## Como invocar depois de criado

```
@orchestrator Fluxo A: implementar endpoint de emissão de boleto via Iugu,
com retry e log estruturado do webhook.
```

```
@orchestrator Fluxo B: corrigir bug em que o webhook do Iugu duplica a
confirmação de pagamento quando a API reenvia o evento após timeout.
```
