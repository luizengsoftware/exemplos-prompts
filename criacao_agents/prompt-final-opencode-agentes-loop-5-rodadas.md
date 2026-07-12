# Revisão e Otimização dos Agentes Locais do OpenCode

Atue como um arquiteto de agentes de IA especializado em OpenCode, engenharia de software, automação de desenvolvimento, agentes especializados, otimização de custos e fluxos iterativos de execução.

Quero que você revise a configuração atual de agentes deste projeto e adapte a estrutura para um modelo de roteamento por responsabilidade, capacidade, complexidade e risco técnico.

A configuração abaixo representa uma **sugestão inicial de modelos**. Ela não deve ser aplicada cegamente: valide os identificadores, a disponibilidade, a compatibilidade com agentes e a ausência de custo adicional no ambiente atual.

Caso existam modelos novos, melhores e igualmente sem custo adicional em qualquer provider já disponível, você poderá recomendá-los e utilizá-los, desde que apresente a justificativa e aguarde minha aprovação antes de alterar os arquivos.

# Requisitos obrigatórios

## 1. Configuração exclusivamente local ao projeto

Toda configuração proposta ou implementada deve permanecer exclusivamente no repositório atual.

Não crie, altere, remova ou sobrescreva configurações globais do OpenCode.

Não modifique:

- `~/.config/opencode/`;
- `~/.opencode/`;
- arquivos globais de agentes;
- arquivos globais de comandos;
- configurações globais de providers;
- arquivos compartilhados entre projetos;
- credenciais ou arquivos de autenticação;
- qualquer arquivo localizado fora da raiz do repositório atual.

Mesmo quando este documento mencionar `opencode.json`, entenda-o como o arquivo **local da raiz deste projeto**, e não como uma configuração global do usuário.

Os agentes, comandos, instruções e regras devem utilizar somente caminhos locais suportados pela versão instalada do OpenCode, como:

```text
<raiz-do-projeto>/
├── opencode.json
├── opencode.jsonc
├── AGENTS.md
└── .opencode/
    ├── agents/
    ├── commands/
    └── ...
```

Antes de propor alterações:

1. identifique a versão instalada do OpenCode;
2. confirme os caminhos locais oficialmente suportados por essa versão;
3. identifique a raiz real do repositório;
4. diferencie configurações locais e globais;
5. confirme que todos os arquivos propostos ficarão dentro do projeto;
6. não altere nenhum arquivo antes da minha aprovação.

A solução deverá:

- funcionar somente neste projeto;
- ser versionável no Git;
- não alterar o comportamento de outros projetos;
- não depender da criação de agentes ou comandos globais;
- não adicionar credenciais ao repositório;
- preservar autenticações já configuradas fora do projeto;
- utilizar apenas referências seguras aos modelos disponíveis.

## 2. Providers inicialmente permitidos

A sugestão inicial considera estes providers:

```json
{
  "enabled_providers": ["openai", "google", "opencode"]
}
```

Antes de aplicar essa configuração:

1. verifique se `enabled_providers` ainda é suportado pela versão instalada;
2. verifique se a documentação atual recomenda outro mecanismo;
3. utilize o mecanismo mais atual e oficialmente suportado;
4. mantenha a configuração somente no `opencode.json` ou `opencode.jsonc` local do projeto;
5. não altere autenticações globais;
6. não copie tokens ou credenciais para o repositório.

Caso a versão instalada utilize outro mecanismo, proponha a configuração local equivalente e explique a diferença.

## 3. Somente modelos sem custo adicional

Considere modelos de qualquer provider já disponível e autenticado no OpenCode, incluindo:

- OpenCode;
- OpenAI;
- Google Gemini;
- Anthropic;
- GitHub Copilot;
- modelos locais;
- outros providers existentes no ambiente.

Porém, somente poderão ser recomendados ou configurados modelos cuja utilização não gere custo adicional no cenário atual.

Podem ser elegíveis:

1. modelos gratuitos fornecidos pelo OpenCode;
2. modelos incluídos em uma assinatura já existente;
3. modelos acessados por OAuth sem cobrança adicional por API;
4. modelos com franquia gratuita recorrente;
5. modelos locais sem cobrança por token ou requisição;
6. novos modelos gratuitos ou incluídos que se tornem disponíveis futuramente.

Não trate automaticamente como gratuito:

- um modelo apenas porque aparece em `opencode models`;
- um modelo apenas porque o provider está conectado;
- APIs cobradas por token;
- saldo pré-pago;
- créditos promocionais;
- trials;
- modelos temporariamente gratuitos sem registrar essa condição;
- modelos incluídos em uma assinatura diferente da autenticação realmente utilizada;
- nomes contendo `free` sem confirmação.

Uma assinatura ChatGPT, Gemini, Claude, Copilot ou similar não torna automaticamente a API correspondente gratuita.

## 4. Sugestão inicial de modelos por agente

Use a configuração abaixo como ponto de partida para a análise.

### Agentes principais de código e análise

| Agente | Modelo inicialmente sugerido |
|---|---|
| `architect` | `opencode/deepseek-v4-flash-free` |
| `developer` | `opencode/deepseek-v4-flash-free` |
| `reviewer` | `opencode/deepseek-v4-flash-free` |
| `security-reviewer` | `opencode/deepseek-v4-flash-free` |

### Agentes rápidos e de apoio

| Agente | Modelo inicialmente sugerido |
|---|---|
| `orchestrator` | `openai/gpt-5.4-mini-fast` |
| `explorer` | `openai/gpt-5.4-mini-fast` |
| `documentation` | `openai/gpt-5.4-mini` |
| `test-engineer` | `openai/gpt-5.4-mini` |

Esses identificadores são apenas candidatos.

Antes de utilizá-los:

1. execute o mecanismo oficial de listagem de modelos;
2. confirme o identificador exato no formato `provider/model`;
3. confirme que o modelo está disponível no ambiente atual;
4. confirme que suporta ferramentas;
5. confirme que pode ser atribuído a agentes;
6. confirme que não gera custo adicional;
7. confirme eventuais limites de uso;
8. confirme se a gratuidade é permanente, recorrente ou temporária.

Use, quando disponível:

```bash
opencode models
```

E, para filtrar por provider:

```bash
opencode models openai
opencode models google
opencode models opencode
```

Se algum identificador sugerido não existir exatamente dessa forma, não o invente e não o configure.

## 5. Liberdade para recomendar modelos melhores

A configuração sugerida não é definitiva.

Caso encontre modelos mais novos e melhores que também não gerem custo adicional, poderá recomendar a substituição.

Considere melhorias em:

- qualidade de código;
- aderência a instruções;
- tool calling;
- raciocínio;
- tamanho de contexto;
- velocidade;
- estabilidade;
- limite de uso;
- disponibilidade;
- qualidade para revisão;
- qualidade para segurança;
- qualidade para múltiplos arquivos.

Ao sugerir uma substituição, apresente:

| Campo | Informação |
|---|---|
| Agente | Papel afetado |
| Modelo sugerido originalmente | Modelo deste documento |
| Novo modelo recomendado | Identificador exato |
| Provider | Provider real |
| Custo adicional | Deve ser zero |
| Limites | Franquia, requests ou contexto |
| Justificativa | Por que é melhor |
| Evidência | Origem da confirmação |
| Risco | Eventuais limitações |

Não substitua automaticamente o modelo antes da minha aprovação.

## 6. Proibição de fallback pago

Não configure fallback automático para modelos que possam gerar cobrança.

Caso um modelo gratuito ou incluído:

- fique indisponível;
- alcance o limite;
- seja removido;
- deixe de ser gratuito;
- não suporte a tarefa;
- não tenha capacidade suficiente;

o fluxo deverá:

1. interromper a execução;
2. informar o motivo;
3. apresentar os modelos gratuitos ou incluídos ainda disponíveis;
4. solicitar decisão humana;
5. nunca migrar silenciosamente para um modelo pago.

# Objetivo da arquitetura de agentes

Organizar os agentes locais deste projeto em uma estrutura semelhante a:

- **Orchestrator**: recebe a solicitação, classifica a tarefa e coordena os demais agentes;
- **Explorer**: pesquisa o repositório e localiza arquivos, símbolos, referências, fluxos e padrões;
- **Developer**: implementa features, correções e refatorações;
- **Test Engineer**: cria, executa e melhora testes;
- **Architect**: avalia decisões arquiteturais, dependências e mudanças transversais;
- **Reviewer**: revisa o diff e identifica bugs, regressões e problemas de qualidade;
- **Security Reviewer**: avalia autenticação, autorização, privacidade, isolamento e exposição de dados;
- **Documentation Agent**: atualiza documentação técnica, README, ADRs e instruções.

Essa lista é uma referência.

Primeiro, analise os agentes locais existentes e determine:

- quais devem ser mantidos;
- quais devem ser alterados;
- quais estão duplicados;
- quais podem ser consolidados;
- quais podem ser removidos;
- quais agentes adicionais realmente agregariam valor;
- quais responsabilidades precisam ser separadas;
- quais papéis podem compartilhar o mesmo modelo;
- quais papéis podem ser unidos para evitar complexidade desnecessária.

Evite criar agentes em excesso.

# Etapa 1 — Identificar projeto, versão e escopo

Antes de qualquer alteração:

1. identifique a raiz do repositório atual;
2. identifique a versão instalada do OpenCode;
3. liste os arquivos locais relacionados ao OpenCode;
4. confirme os caminhos locais suportados;
5. identifique se existe `opencode.json`, `opencode.jsonc`, `AGENTS.md` ou `.opencode/`;
6. diferencie configurações locais e globais;
7. não altere nenhum arquivo.

Informe explicitamente:

- raiz identificada;
- versão do OpenCode;
- caminhos locais suportados;
- mecanismo atual de providers;
- quais arquivos serão somente lidos;
- quais arquivos poderão ser alterados após aprovação.

# Etapa 2 — Inspecionar a configuração local atual

Analise, quando existirem:

- `opencode.json`;
- `opencode.jsonc`;
- `.opencode/`;
- `.opencode/agents/`;
- `.opencode/commands/`;
- `.opencode/skills/`;
- `AGENTS.md`;
- arquivos de instruções;
- documentação arquitetural;
- convenções do projeto;
- configurações locais relacionadas aos agentes.

Configurações globais podem ser consultadas somente para compreender:

- heranças;
- providers autenticados;
- modelos disponíveis;
- comportamento da instalação;
- limites conhecidos.

Não altere configurações globais.

Para cada agente local atual, informe:

1. nome;
2. caminho do arquivo;
3. modo, como `primary`, `subagent` ou equivalente;
4. modelo configurado;
5. provider do modelo;
6. ferramentas disponíveis;
7. permissões de leitura, escrita, edição e execução;
8. responsabilidade atual;
9. sobreposição com outros agentes;
10. riscos ou problemas;
11. adequação do modelo ao papel;
12. recomendação de manutenção, alteração, fusão ou remoção.

# Etapa 3 — Descobrir providers e modelos disponíveis

Utilize os comandos e mecanismos oficiais da instalação atual.

Consulte:

- `opencode models`;
- providers autenticados;
- modelos configurados localmente;
- modelos herdados de autenticações existentes;
- integrações OAuth;
- modelos gratuitos do OpenCode;
- modelos gratuitos ou incluídos da OpenAI;
- modelos gratuitos ou incluídos do Google;
- demais providers disponíveis.

Não invente identificadores.

Para cada modelo encontrado, registre:

- provider;
- identificador exato;
- nome apresentado;
- mecanismo de autenticação;
- origem da disponibilidade;
- forma de cobrança;
- se é gratuito;
- se está incluído em assinatura;
- se possui franquia gratuita;
- se utiliza créditos;
- se é promocional;
- se é gratuito por tempo limitado;
- limite de requisições;
- contexto;
- suporte a ferramentas;
- suporte a raciocínio;
- qualidade para programação;
- velocidade;
- limitações;
- tarefas mais adequadas.

Não revele tokens, chaves ou segredos.

# Etapa 4 — Validar custo e elegibilidade

Classifique cada modelo em:

| Classificação | Definição |
|---|---|
| Elegível: gratuito | Não gera cobrança e não depende de assinatura paga |
| Elegível: incluído | Está incluído em assinatura existente, sem custo variável adicional confirmado |
| Elegível: local | Executado localmente, sem cobrança por token ou requisição |
| Elegível com limite | Gratuito ou incluído, sujeito a franquia ou limite recorrente |
| Elegível temporário | Gratuito por tempo limitado, com data ou condição registrada |
| Não elegível: pago por uso | Gera cobrança por token, requisição, crédito ou saldo |
| Não elegível: promocional | Depende de trial, bônus ou crédito temporário não recorrente |
| Não elegível: custo não confirmado | Não foi possível confirmar a ausência de cobrança |
| Não elegível: indisponível | Documentado, mas não acessível no ambiente atual |

Somente estas classificações poderão ser recomendadas:

- `Elegível: gratuito`;
- `Elegível: incluído`;
- `Elegível: local`;
- `Elegível com limite`;
- `Elegível temporário`, desde que exista alerta explícito e nenhuma cobrança automática.

Para cada conclusão, apresente a evidência:

- saída do OpenCode;
- configuração local;
- mecanismo de autenticação;
- documentação oficial;
- informação oficial do provider;
- condição não confirmada.

# Etapa 5 — Avaliar os modelos sugeridos

Avalie especificamente:

## `opencode/deepseek-v4-flash-free`

Verifique:

- se o ID exato existe;
- se pertence ao provider `opencode`;
- se está gratuito atualmente;
- se a gratuidade é temporária;
- se possui limite;
- se suporta tool calling;
- se é adequado para implementação;
- se é adequado para arquitetura;
- se é adequado para revisão;
- se é adequado para segurança;
- se deveria ser utilizado em todos os quatro papéis ou apenas em alguns.

## `openai/gpt-5.4-mini-fast`

Verifique:

- se o ID exato existe;
- se está acessível pela autenticação OpenAI atual;
- se o acesso utiliza OAuth ou API;
- se gera cobrança separada;
- se está incluído em uma assinatura;
- se suporta tool calling;
- se é adequado para `orchestrator` e `explorer`.

## `openai/gpt-5.4-mini`

Verifique:

- se o ID exato existe;
- se está acessível pela autenticação atual;
- se gera cobrança adicional;
- se suporta tool calling;
- se é adequado para documentação e testes.

Caso qualquer modelo não exista, tenha custo não confirmado ou não seja adequado, proponha uma alternativa elegível.

# Etapa 6 — Avaliar modelos novos ou superiores

Procure também modelos novos ou melhores nos providers disponíveis.

Avalie-os para:

- exploração;
- implementação;
- testes;
- documentação;
- arquitetura;
- revisão;
- segurança;
- contexto extenso;
- múltiplos arquivos;
- execução com ferramentas.

Não escolha automaticamente o modelo mais forte para todos os agentes.

Considere:

- qualidade;
- custo adicional;
- limite;
- estabilidade;
- velocidade;
- contexto;
- tool calling;
- confiabilidade;
- disponibilidade.

# Etapa 7 — Criar a matriz final

Apresente:

| Papel | Modelo sugerido inicialmente | Modelo recomendado | Provider | Classificação de custo | Alternativa elegível | Limites | Motivo |
|---|---|---|---|---|---|---|---|
| Orchestrator | `openai/gpt-5.4-mini-fast` | ... | ... | ... | ... | ... | ... |
| Explorer | `openai/gpt-5.4-mini-fast` | ... | ... | ... | ... | ... | ... |
| Developer | `opencode/deepseek-v4-flash-free` | ... | ... | ... | ... | ... | ... |
| Test Engineer | `openai/gpt-5.4-mini` | ... | ... | ... | ... | ... | ... |
| Architect | `opencode/deepseek-v4-flash-free` | ... | ... | ... | ... | ... | ... |
| Reviewer | `opencode/deepseek-v4-flash-free` | ... | ... | ... | ... | ... | ... |
| Security Reviewer | `opencode/deepseek-v4-flash-free` | ... | ... | ... | ... | ... | ... |
| Documentation | `openai/gpt-5.4-mini` | ... | ... | ... | ... | ... | ... |

Regras:

- utilize apenas IDs reais;
- não apresente modelo pago como fallback;
- não confunda assinatura com API gratuita;
- não confunda crédito promocional com gratuidade;
- informe quando a gratuidade for temporária;
- informe quando um papel exigir revisão humana;
- não force diversidade de modelos quando um único modelo elegível for melhor.

# Etapa 8 — Propor o roteamento automático

Fluxo de referência:

```text
Solicitação
    ↓
Orchestrator
    ↓
Explorer
    ↓
Architect, quando necessário
    ↓
Developer
    ↓
Test Engineer
    ↓
Reviewer ou Security Reviewer, quando necessário
```

## Baixa complexidade

Exemplos:

- ajustes textuais;
- documentação;
- busca de referências;
- alterações mecânicas;
- testes simples;
- pequenas correções.

Fluxo:

```text
Orchestrator → Explorer ou Developer → validação
```

## Média complexidade

Exemplos:

- novo endpoint;
- CRUD;
- regra de negócio;
- integração comum;
- alteração em múltiplas camadas;
- feature envolvendo vários arquivos;
- correção com testes.

Fluxo:

```text
Orchestrator → Explorer → Developer → Test Engineer → Reviewer
```

## Alta complexidade ou alto risco

Exemplos:

- autenticação;
- autorização;
- privacidade;
- Chinese Wall;
- segregação entre tenants;
- dados financeiros;
- migrations;
- concorrência;
- processamento assíncrono;
- alteração arquitetural;
- incidente de produção;
- múltiplos módulos afetados.

Fluxo:

```text
Orchestrator
    → Explorer
    → Architect
    → aprovação humana, quando necessário
    → Developer
    → Test Engineer
    → Security Reviewer
    → Reviewer
```

Caso nenhum modelo elegível seja suficientemente confiável, interrompa o fluxo e solicite revisão humana.

# Etapa 9 — Execução em loop com limite de cinco rodadas

Configure o fluxo para trabalhar de forma iterativa até que a demanda seja concluída ou até atingir o limite máximo de cinco rodadas.

Cada rodada deverá seguir, quando aplicável:

```text
1. Orchestrator classifica o estado atual
2. Explorer ou Architect identifica lacunas
3. Developer implementa ou corrige
4. Test Engineer executa e analisa testes
5. Reviewer e/ou Security Reviewer valida o resultado
6. Orchestrator decide se a demanda foi concluída ou se precisa de nova rodada
```

## Regras do loop

- limite máximo: **5 rodadas**;
- uma rodada só deve ocorrer quando houver uma ação concreta a executar;
- não repita a mesma tentativa sem alterar a abordagem;
- registre o objetivo de cada rodada;
- registre o resultado obtido;
- registre os erros ou lacunas restantes;
- altere a estratégia quando uma rodada falhar;
- não esconda falhas;
- não declare conclusão sem evidência;
- não ultrapasse cinco rodadas;
- não use modelo pago para tentar completar após o limite;
- não reinicie a contagem sem autorização humana.

## Critério de conclusão

A demanda poderá ser considerada concluída somente quando:

- o comportamento esperado estiver implementado;
- os critérios de aceite estiverem atendidos;
- build e testes relevantes tiverem sido executados;
- não houver erro conhecido bloqueante;
- o diff tiver sido revisado;
- riscos residuais tiverem sido informados;
- documentação necessária tiver sido atualizada.

## Encerramento antes de cinco rodadas

Encerre antes do limite quando a demanda estiver 100% concluída e validada.

## Encerramento ao atingir cinco rodadas

Ao atingir a quinta rodada, interrompa obrigatoriamente.

Caso a demanda não esteja 100% concluída, apresente um alerta explícito:

> **ATENÇÃO: a demanda não foi concluída integralmente após o limite de 5 rodadas.**

Informe:

1. percentual estimado de conclusão;
2. itens concluídos;
3. itens pendentes;
4. erros ou bloqueios;
5. testes que passaram;
6. testes que falharam;
7. riscos conhecidos;
8. arquivos afetados;
9. recomendação para a próxima ação;
10. se é necessária intervenção humana.

Não informe 100% de conclusão quando existirem pendências, testes falhando, critérios de aceite não validados ou riscos bloqueantes.

# Etapa 10 — Definir regras objetivas

Proponha regras como:

- acionar `architect` quando houver migration;
- acionar `architect` quando mais de três módulos forem afetados;
- acionar `security-reviewer` quando houver autenticação ou autorização;
- acionar `security-reviewer` quando houver visibilidade por usuário, empresa, perfil, tenant, custodiante ou escriturador;
- acionar `reviewer` quando mais de dez arquivos forem modificados;
- acionar `test-engineer` quando uma regra de negócio for alterada;
- impedir `explorer` de editar arquivos;
- impedir `architect`, `reviewer` e `security-reviewer` de alterar diretamente a implementação;
- permitir escrita somente aos agentes responsáveis pela implementação e documentação;
- exigir aprovação humana antes de migrations destrutivas;
- exigir aprovação humana antes de alterações irreversíveis;
- nunca realizar fallback silencioso para modelo pago;
- informar quando um modelo atingir seu limite;
- revalidar modelos gratuitos temporários periodicamente.

Adapte os limites à arquitetura real do projeto.

# Etapa 11 — Reduzir consumo de contexto

Inclua regras para:

- pesquisar antes de abrir arquivos completos;
- localizar símbolos e referências antes de ler módulos;
- ignorar `bin`, `obj`, `node_modules` e artefatos de build;
- ignorar arquivos gerados;
- não ler outros projetos da máquina;
- não reenviar o repositório inteiro entre agentes;
- compartilhar um resumo estruturado da exploração;
- enviar apenas arquivos e trechos relevantes;
- priorizar diffs;
- evitar repetir logs completos;
- limitar respostas excessivamente longas;
- reutilizar `AGENTS.md`, `ARCHITECTURE.md` e ADRs;
- respeitar limites dos modelos;
- utilizar modelos rápidos para tarefas mecânicas;
- reservar os modelos mais capazes para tarefas críticas.

# Etapa 12 — Propor a estrutura local final

Apresente:

1. estrutura final de agentes;
2. responsabilidades;
3. modelo atribuído a cada agente;
4. provider;
5. classificação de custo;
6. modelo alternativo elegível;
7. permissões;
8. ferramentas;
9. critérios de acionamento;
10. fluxo de delegação;
11. regra de loop de até cinco rodadas;
12. arquivos que serão criados;
13. arquivos que serão alterados;
14. arquivos que serão consolidados ou removidos;
15. limitações dos modelos;
16. pontos que exigirão revisão humana;
17. confirmação de que não haverá fallback pago;
18. confirmação de que nenhuma configuração global será alterada.

Exemplo:

```text
<raiz-do-projeto>/
├── opencode.jsonc
├── AGENTS.md
└── .opencode/
    ├── agents/
    │   ├── orchestrator.md
    │   ├── explorer.md
    │   ├── developer.md
    │   ├── test-engineer.md
    │   ├── architect.md
    │   ├── reviewer.md
    │   ├── security-reviewer.md
    │   └── documentation.md
    └── commands/
        ├── feature.md
        ├── fix.md
        ├── feature-critical.md
        ├── review.md
        ├── explore.md
        └── document.md
```

Adapte ao formato suportado pela versão instalada.

# Etapa 13 — Criar comandos locais

Sugira comandos como:

```text
/feature
/fix
/feature-critical
/review
/explore
/document
```

Cada comando deve:

- existir somente neste projeto;
- utilizar agentes locais;
- utilizar apenas modelos elegíveis;
- não interferir em outros projetos;
- não realizar fallback pago;
- utilizar o loop de até cinco rodadas quando houver implementação ou correção;
- solicitar intervenção humana quando nenhum modelo for adequado.

# Etapa 14 — Segurança e versionamento

Não grave no repositório:

- tokens;
- API keys;
- credenciais;
- sessões;
- cookies;
- configurações globais copiadas;
- segredos de providers;
- arquivos de autenticação.

Caso um modelo dependa de autenticação global ou de assinatura existente:

- preserve a autenticação fora do projeto;
- mantenha no projeto somente o identificador do modelo;
- não copie segredos;
- informe a dependência;
- informe o comportamento quando a autenticação não estiver disponível.

Informe eventuais ajustes necessários no `.gitignore`.

# Etapa 15 — Aprovação obrigatória

Antes de editar qualquer arquivo:

1. apresente o diagnóstico;
2. apresente os providers e modelos encontrados;
3. valide especificamente os modelos sugeridos;
4. classifique custo e elegibilidade;
5. apresente modelos novos ou superiores encontrados;
6. apresente a matriz final;
7. apresente a arquitetura proposta;
8. apresente a estratégia de loop de até cinco rodadas;
9. apresente os caminhos que serão alterados;
10. confirme que todos estão dentro do projeto;
11. confirme que não haverá fallback pago;
12. confirme que nenhuma configuração global será modificada;
13. aguarde minha aprovação.

Somente depois da aprovação:

- crie ou altere arquivos locais;
- preserve configurações úteis;
- não remova arquivos sem justificar;
- valide sintaxe;
- valide IDs;
- valide disponibilidade;
- valide ausência de custo adicional;
- valide carregamento dos agentes;
- valide comandos;
- valide o comportamento do loop;
- apresente o diff;
- informe como testar;
- confirme que nenhum fallback pago foi configurado;
- confirme que nenhum arquivo global foi alterado.

# Critérios de qualidade

A solução deve:

- considerar OpenCode, OpenAI, Google e outros providers disponíveis;
- usar a configuração sugerida como ponto de partida;
- aceitar modelos novos e melhores sem custo adicional;
- não utilizar IDs inventados;
- não confundir assinatura com API gratuita;
- não usar créditos promocionais como base permanente;
- não gerar cobrança sem aprovação;
- não possuir fallback pago;
- funcionar na versão instalada;
- ser totalmente local ao projeto;
- ser versionável no Git;
- não conter credenciais;
- minimizar leituras repetidas;
- separar responsabilidades;
- evitar agentes duplicados;
- exigir revisão humana em tarefas críticas;
- reconhecer limites e gratuidade temporária;
- executar demandas em loop por no máximo cinco rodadas;
- alertar claramente quando não atingir 100% do esperado;
- não alterar outros projetos.

# Formato da resposta inicial

Responda inicialmente com:

1. **Raiz do projeto identificada**
2. **Versão do OpenCode**
3. **Mecanismo de configuração de providers suportado**
4. **Configuração local encontrada**
5. **Diagnóstico dos agentes atuais**
6. **Providers autenticados ou disponíveis**
7. **Catálogo de modelos encontrado**
8. **Validação dos modelos sugeridos**
9. **Validação de custo e forma de acesso**
10. **Modelos elegíveis**
11. **Novos modelos superiores encontrados**
12. **Modelos descartados e motivos**
13. **Matriz recomendada por papel**
14. **Arquitetura local proposta**
15. **Regras de roteamento**
16. **Estratégia de loop de até cinco rodadas**
17. **Critérios de conclusão**
18. **Limitações, franquias e gratuidade temporária**
19. **Etapas que exigirão revisão humana**
20. **Estrutura de arquivos proposta**
21. **Plano de alterações**
22. **Confirmação de isolamento por projeto**
23. **Confirmação de ausência de fallback pago**
24. **Aguardando aprovação**

Na confirmação final, declare explicitamente:

- quais arquivos locais pretende modificar;
- que nenhum arquivo global será alterado;
- que nenhuma credencial será adicionada ao repositório;
- quais modelos sugeridos foram validados;
- quais modelos foram substituídos e por quê;
- quais modelos novos e melhores foram encontrados;
- qual evidência confirma a ausência de custo adicional;
- quais modelos têm gratuidade temporária ou limites;
- como funcionará o loop de até cinco rodadas;
- que haverá alerta explícito quando a demanda não atingir 100% do esperado;
- que nenhum fallback pago será configurado;
- que a configuração valerá somente para o projeto atual.

Não realize alterações antes da minha aprovação.
