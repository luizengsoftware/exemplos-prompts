# Revisão e Otimização dos Agentes Locais do OpenCode com Modelos Gratuitos ou Sem Custo Adicional

Atue como um arquiteto de agentes de IA especializado em OpenCode, engenharia de software, automação de desenvolvimento, agentes especializados e otimização de custos.

Quero que você revise a configuração atual de agentes deste projeto e proponha uma estratégia de roteamento por responsabilidade, complexidade, capacidade do modelo e risco técnico.

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
5. confirme que todos os arquivos propostos ficarão dentro do projeto.

A solução deverá:

- funcionar somente neste projeto;
- ser versionável no Git;
- não alterar o comportamento de outros projetos;
- não depender da criação de agentes ou comandos globais;
- não adicionar credenciais ao repositório;
- preservar autenticações já configuradas fora do projeto;
- utilizar apenas referências seguras aos modelos disponíveis.

## 2. Considerar modelos gratuitos ou sem custo adicional disponíveis no ambiente atual

Não limite a análise apenas aos modelos fornecidos diretamente pelo OpenCode.

Considere modelos de qualquer provider suportado e autenticado no OpenCode, incluindo, entre outros:

- OpenCode;
- OpenCode Zen, quando houver modelo realmente gratuito;
- OpenAI;
- Anthropic;
- Google Gemini;
- GitHub Copilot;
- Amazon Bedrock;
- Azure OpenAI;
- OpenRouter;
- Groq;
- Cloudflare AI Gateway;
- Vercel AI Gateway;
- modelos locais;
- Ollama;
- LM Studio;
- outros providers disponíveis no ambiente.

Entretanto, somente poderão ser recomendados como **gratuitos ou sem custo adicional** os modelos cuja condição de uso tenha sido confirmada no ambiente atual.

Podem ser considerados elegíveis:

1. modelos marcados como gratuitos pelo OpenCode;
2. modelos gratuitos oferecidos por providers externos;
3. modelos incluídos em uma assinatura já existente, sem cobrança adicional por token para o uso atual;
4. modelos acessados por autenticação de plano já contratado, quando o uso não consumir créditos adicionais;
5. modelos locais que não gerem cobrança por requisição ou token;
6. modelos com franquia gratuita recorrente, desde que seus limites sejam claramente informados;
7. modelos gratuitos acessados por integrações ou plugins oficialmente suportados pelo ambiente atual.

Exemplos de situações potencialmente elegíveis, desde que confirmadas:

- uso de ChatGPT Plus ou Pro por integração compatível com OpenCode, sem cobrança de API separada;
- uso de plano Gemini já disponível por autenticação compatível;
- uso de GitHub Copilot já contratado;
- uso de modelos gratuitos do OpenCode;
- uso de modelos locais via Ollama;
- uso de free tier recorrente de algum provider.

Não considere automaticamente como gratuito apenas porque:

- o modelo aparece na lista;
- o provider está autenticado;
- existe saldo disponível;
- há créditos promocionais;
- existe um trial;
- o preço é baixo;
- o usuário já possui uma assinatura;
- o modelo tem a palavra `free` no nome.

## 3. Conceito de “sem custo adicional”

Para este trabalho, classifique como **sem custo adicional** somente quando o uso do modelo não gerar cobrança variável adicional no cenário atual.

Uma assinatura já paga poderá ser considerada sem custo adicional apenas quando:

- a integração utilizada estiver incluída nessa assinatura;
- não houver cobrança separada por API;
- não houver consumo de créditos pagos adicionais;
- os limites de uso estiverem identificados;
- essa condição puder ser confirmada pelo ambiente, documentação oficial ou configuração instalada.

Exemplo:

- uma conta ChatGPT Plus não torna automaticamente a API da OpenAI gratuita;
- uma conta Gemini não torna automaticamente toda API do Google gratuita;
- uma assinatura Copilot pode permitir uso incluído, mas seus limites devem ser verificados;
- créditos promocionais não equivalem a gratuidade permanente;
- OpenCode Zen normalmente pode envolver cobrança e não deve ser tratado como gratuito sem confirmação.

## 4. Proibição de fallback que gere cobrança

Não configure fallback automático para qualquer modelo que possa gerar cobrança.

Caso o modelo gratuito ou incluído fique indisponível, alcance o limite de uso ou não seja adequado para a tarefa:

1. interrompa o fluxo;
2. informe o motivo;
3. apresente as alternativas disponíveis;
4. solicite decisão humana antes de utilizar um modelo pago.

Nunca migre silenciosamente para:

- API cobrada por token;
- saldo pré-pago;
- créditos do provider;
- modelo premium;
- provider alternativo pago;
- rota de cobrança não confirmada.

# Objetivo

Organizar os agentes locais deste projeto em uma estrutura semelhante a:

- **Orchestrator**: recebe a solicitação, classifica a tarefa e coordena os demais agentes;
- **Explorer**: pesquisa o repositório e localiza arquivos, símbolos, referências, fluxos e padrões;
- **Developer**: implementa features, correções e refatorações;
- **Test Engineer**: cria, executa e melhora testes;
- **Architect**: avalia decisões arquiteturais, dependências e mudanças transversais;
- **Reviewer**: revisa o diff e identifica bugs, regressões e problemas de qualidade;
- **Security Reviewer**: avalia autenticação, autorização, privacidade, isolamento e exposição de dados;
- **Documentation Agent**: atualiza documentação técnica, README, ADRs e instruções.

Essa lista é apenas uma referência.

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

# Etapa 1 — Identificar o projeto e a versão do OpenCode

Antes de qualquer alteração:

1. identifique a raiz do repositório atual;
2. identifique a versão instalada do OpenCode;
3. liste os arquivos locais relacionados ao OpenCode;
4. confirme os caminhos locais suportados por essa versão;
5. identifique se existe `opencode.json`, `opencode.jsonc`, `AGENTS.md` ou `.opencode/`;
6. diferencie configurações locais e globais;
7. não altere nenhum arquivo.

Informe explicitamente:

- a raiz identificada;
- a versão do OpenCode;
- os caminhos locais suportados;
- quais arquivos serão apenas lidos;
- quais arquivos poderão ser alterados somente após aprovação.

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

Não altere arquivos nesta etapa.

# Etapa 3 — Descobrir os modelos realmente disponíveis

Utilize os comandos e mecanismos oficiais da instalação atual para listar os modelos e providers disponíveis.

Consulte, quando aplicável:

- catálogo de modelos carregado pelo OpenCode;
- providers autenticados;
- modelos configurados no projeto;
- modelos configurados globalmente;
- integrações de assinatura;
- plugins de autenticação;
- modelos locais;
- modelos incluídos pelo próprio OpenCode.

Não invente nomes ou identificadores.

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
- se é local;
- limite de requisições, quando disponível;
- contexto;
- suporte a ferramentas;
- suporte a raciocínio;
- qualidade para programação;
- velocidade;
- limitações;
- tarefas mais adequadas.

Não revele tokens, chaves ou segredos.

# Etapa 4 — Validar custo e elegibilidade

Classifique cada modelo em uma destas categorias:

| Classificação | Definição |
|---|---|
| Elegível: gratuito | Não gera cobrança e não depende de assinatura paga |
| Elegível: incluído | Está incluído em assinatura ou plano existente, sem custo variável adicional confirmado |
| Elegível: local | Executado localmente, sem cobrança por token ou requisição |
| Elegível com limite | Gratuito ou incluído, mas sujeito a franquia ou limite recorrente conhecido |
| Não elegível: pago por uso | Gera cobrança por token, requisição, crédito ou saldo |
| Não elegível: promocional | Depende de trial, bônus ou crédito temporário |
| Não elegível: custo não confirmado | Não foi possível confirmar se haverá cobrança |
| Não elegível: indisponível | Está documentado, mas não está acessível no ambiente atual |

Somente modelos classificados como:

- `Elegível: gratuito`;
- `Elegível: incluído`;
- `Elegível: local`;
- `Elegível com limite`;

poderão ser recomendados.

Um modelo com gratuidade temporária ou custo não confirmado não poderá ser configurado automaticamente.

Para cada conclusão de custo, informe a evidência utilizada:

- indicação apresentada pelo OpenCode;
- configuração local;
- mecanismo de autenticação;
- documentação oficial;
- informação do provider;
- limitação que não pôde ser confirmada.

# Etapa 5 — Avaliar a capacidade dos modelos elegíveis

Avalie os modelos elegíveis para:

- exploração do repositório;
- busca de símbolos e referências;
- leitura de código;
- implementação;
- correções;
- refatorações;
- geração de testes;
- execução e análise de testes;
- documentação;
- planejamento;
- arquitetura;
- code review;
- segurança;
- tarefas com ferramentas;
- tarefas de contexto extenso;
- tarefas com múltiplos arquivos;
- tarefas críticas.

Considere, além da qualidade:

- limite de uso;
- estabilidade;
- velocidade;
- tamanho de contexto;
- qualidade de tool calling;
- capacidade de seguir instruções;
- confiabilidade em alterações de código;
- disponibilidade no ambiente;
- risco de atingir a franquia;
- custo efetivamente incremental.

Não escolha automaticamente o modelo mais forte para todos os agentes.

Caso exista apenas um modelo elegível suficientemente capaz, ele poderá ser reutilizado por múltiplos agentes com:

- prompts diferentes;
- ferramentas diferentes;
- permissões diferentes;
- limites diferentes;
- regras de acionamento diferentes.

# Etapa 6 — Criar a matriz de agentes e modelos

Apresente uma matriz como:

| Papel | Modelo principal | Provider | Classificação de custo | Modelo alternativo | Limites | Motivo |
|---|---|---|---|---|---|---|
| Orchestrator | ... | ... | ... | ... | ... | ... |
| Explorer | ... | ... | ... | ... | ... | ... |
| Developer | ... | ... | ... | ... | ... | ... |
| Test Engineer | ... | ... | ... | ... | ... | ... |
| Architect | ... | ... | ... | ... | ... | ... |
| Reviewer | ... | ... | ... | ... | ... | ... |
| Security Reviewer | ... | ... | ... | ... | ... | ... |
| Documentation | ... | ... | ... | ... | ... | ... |

Regras:

- utilize somente identificadores reais;
- não apresente modelos pagos como fallback automático;
- não confunda assinatura com API gratuita;
- não confunda créditos promocionais com gratuidade;
- não recomende modelo com custo não confirmado;
- informe quando um papel exigir revisão humana;
- informe quando um modelo tiver limite baixo demais para determinado papel.

Caso não exista modelo elegível adequado para um papel, registre:

> Sem modelo gratuito ou sem custo adicional confirmado para este papel.

Nesse caso, proponha uma destas opções:

- manter o papel com capacidade limitada;
- unir o papel a outro agente;
- exigir revisão humana;
- não criar esse agente;
- interromper o fluxo quando ele for necessário;
- solicitar autorização explícita para uso de modelo pago, sem configurá-lo automaticamente.

# Etapa 7 — Propor o roteamento automático

Crie uma estratégia em que o agente principal classifique a tarefa e delegue automaticamente.

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

Todos os agentes devem utilizar somente modelos elegíveis.

## Baixa complexidade

Exemplos:

- ajustes textuais;
- documentação;
- busca de referências;
- alterações mecânicas;
- testes simples;
- pequenas correções localizadas.

Fluxo sugerido:

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
- feature envolvendo vários arquivos relacionados;
- correção com testes.

Fluxo sugerido:

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
- mudança transversal;
- múltiplos módulos afetados.

Fluxo sugerido:

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

Caso os modelos elegíveis não sejam suficientemente confiáveis para uma decisão crítica, interrompa o fluxo e solicite revisão humana.

# Etapa 8 — Definir regras objetivas

Proponha regras como:

- acionar `architect` quando houver migration;
- acionar `architect` quando mais de três módulos ou projetos forem afetados;
- acionar `security-reviewer` quando houver autenticação ou autorização;
- acionar `security-reviewer` quando houver visibilidade por usuário, empresa, perfil, tenant, custodiante ou escriturador;
- acionar `reviewer` quando mais de dez arquivos forem modificados;
- acionar `test-engineer` sempre que uma regra de negócio for alterada;
- impedir `explorer` de editar arquivos;
- impedir `architect`, `reviewer` e `security-reviewer` de editar diretamente a implementação;
- permitir escrita somente aos agentes responsáveis pela implementação e documentação;
- exigir aprovação humana antes de migrations destrutivas;
- exigir aprovação humana antes de alterações irreversíveis;
- exigir aprovação humana quando nenhum modelo elegível tiver capacidade adequada;
- nunca realizar fallback silencioso para modelo pago;
- nunca utilizar modelo cujo custo não tenha sido confirmado;
- informar quando um limite gratuito ou incluído for atingido.

Adapte os limites à arquitetura real deste projeto.

# Etapa 9 — Reduzir consumo de contexto

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
- respeitar limites dos modelos gratuitos ou incluídos;
- interromper a tarefa quando o limite for atingido;
- não migrar automaticamente para modelo pago;
- utilizar modelos menores em tarefas mecânicas;
- reservar os modelos mais capazes para arquitetura, segurança e revisão crítica.

# Etapa 10 — Propor a estrutura local final

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
11. arquivos que serão criados;
12. arquivos que serão alterados;
13. arquivos que serão consolidados ou removidos;
14. limitações dos modelos;
15. pontos que exigirão revisão humana;
16. confirmação de que não haverá fallback pago;
17. confirmação de que nenhuma configuração global será alterada.

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

Adapte a estrutura ao formato realmente suportado pela versão instalada.

# Etapa 11 — Criar comandos locais

Sugira comandos locais como:

```text
/feature
/fix
/feature-critical
/review
/explore
/document
```

Exemplos:

```text
/feature Criar endpoint para cancelamento de ordens
```

```text
/fix Corrigir o cálculo de juros da liquidação
```

```text
/feature-critical Corrigir acesso indevido entre custodiantes
```

```text
/review Revisar as alterações antes do pull request
```

Cada comando deve:

- existir somente neste projeto;
- utilizar agentes locais;
- utilizar apenas modelos elegíveis;
- não interferir em outros projetos;
- não realizar fallback para modelo pago;
- solicitar intervenção humana quando nenhum modelo elegível for adequado.

# Etapa 12 — Segurança e versionamento

As configurações devem ser adequadas para versionamento no Git.

Não grave no repositório:

- tokens;
- API keys;
- credenciais;
- sessões;
- cookies;
- informações privadas;
- configurações globais copiadas;
- segredos de providers;
- arquivos de autenticação.

Caso um modelo dependa de autenticação global ou de assinatura existente:

- preserve a autenticação fora do projeto;
- mantenha no projeto somente o identificador do modelo;
- não copie segredos;
- informe a dependência;
- informe o comportamento esperado quando a autenticação não estiver disponível.

No projeto, armazene somente:

- identificadores dos modelos;
- agentes;
- comandos;
- instruções;
- permissões;
- regras de roteamento;
- documentação não sensível.

Informe ajustes necessários no `.gitignore`.

# Etapa 13 — Aprovação obrigatória antes das alterações

Antes de editar qualquer arquivo:

1. apresente o diagnóstico;
2. apresente os providers e modelos encontrados;
3. classifique o custo e a elegibilidade de cada modelo;
4. apresente a matriz de agentes;
5. apresente a arquitetura proposta;
6. apresente os caminhos que serão alterados;
7. confirme que todos estão dentro do projeto;
8. confirme que somente modelos gratuitos ou sem custo adicional confirmado serão utilizados;
9. confirme que não haverá fallback pago;
10. confirme que nenhuma configuração global será modificada;
11. aguarde minha aprovação.

Somente depois da aprovação:

- crie ou altere arquivos locais;
- preserve configurações úteis existentes;
- não remova arquivos sem justificar;
- valide a sintaxe;
- valide os identificadores dos modelos;
- valide a disponibilidade dos modelos;
- valide a condição de custo;
- valide se os agentes podem ser carregados;
- valide se os comandos são reconhecidos;
- apresente o diff final;
- informe como testar;
- confirme que nenhum fallback pago foi configurado;
- confirme que nenhum arquivo global foi alterado.

# Critérios de qualidade

A solução deve:

- considerar modelos gratuitos ou incluídos de OpenCode, OpenAI, Anthropic, Gemini e demais providers disponíveis;
- não limitar a análise a um único provider;
- não considerar API paga como gratuita por causa de uma assinatura;
- não utilizar trials ou créditos promocionais como base permanente;
- não gerar cobrança variável sem aprovação;
- não possuir fallback automático para modelos pagos;
- funcionar na versão instalada;
- utilizar identificadores reais;
- ser totalmente local ao projeto;
- ser versionável no Git;
- não conter credenciais;
- minimizar leituras repetidas;
- separar responsabilidades;
- evitar agentes duplicados;
- exigir revisão humana em tarefas críticas;
- reconhecer limites de modelos gratuitos ou incluídos;
- não afirmar gratuidade sem evidência;
- não alterar outros projetos.

# Formato da resposta inicial

Responda inicialmente com:

1. **Raiz do projeto identificada**
2. **Versão do OpenCode**
3. **Configuração local encontrada**
4. **Diagnóstico dos agentes atuais**
5. **Providers autenticados ou disponíveis**
6. **Catálogo de modelos encontrado**
7. **Validação de custo e forma de acesso**
8. **Modelos elegíveis**
9. **Modelos descartados e motivos**
10. **Matriz recomendada por papel**
11. **Arquitetura local proposta**
12. **Regras de roteamento**
13. **Limitações e franquias**
14. **Etapas que exigirão revisão humana**
15. **Estrutura de arquivos proposta**
16. **Plano de alterações**
17. **Confirmação de isolamento por projeto**
18. **Confirmação de ausência de fallback pago**
19. **Aguardando aprovação**

Na confirmação final, declare explicitamente:

- quais arquivos locais pretende modificar;
- que nenhum arquivo global será alterado;
- que nenhuma credencial será adicionada ao repositório;
- quais modelos foram classificados como gratuitos, incluídos, locais ou gratuitos com limite;
- quais evidências sustentam essa classificação;
- que modelos pagos, promocionais ou de custo não confirmado não serão utilizados automaticamente;
- que não será configurado fallback para modelos pagos;
- que a configuração valerá somente para o projeto atual.

Não realize alterações antes da minha aprovação.
