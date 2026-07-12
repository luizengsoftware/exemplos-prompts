# Revisão e Otimização dos Agentes Locais do OpenCode com Modelos Gratuitos

Atue como um arquiteto de agentes de IA especializado em OpenCode, engenharia de software, automação de desenvolvimento e otimização do uso de modelos.

Quero que você revise a configuração atual de agentes deste projeto e proponha uma estratégia de roteamento por responsabilidade, complexidade e risco.

# Requisitos obrigatórios

## 1. Configuração exclusivamente local ao projeto

Toda configuração deve ser criada e mantida exclusivamente no repositório atual.

Não crie, altere, remova ou sobrescreva configurações globais do OpenCode.

Não modifique caminhos como:

- `~/.config/opencode/`;
- `~/.opencode/`;
- diretórios globais de agentes;
- diretórios globais de comandos;
- configurações globais de providers;
- arquivos compartilhados entre projetos;
- qualquer arquivo localizado fora da raiz do repositório atual.

Os agentes, comandos e regras devem utilizar somente caminhos locais suportados pela versão instalada do OpenCode, como:

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

Antes de propor alterações, confirme os caminhos oficialmente suportados pela versão instalada para configurações locais por projeto.

A solução deverá:

- funcionar somente neste projeto;
- ser versionável no Git;
- não alterar o comportamento de outros projetos;
- não depender da criação de agentes globais;
- não gravar credenciais no repositório.

## 2. Utilizar exclusivamente modelos gratuitos oferecidos pelo próprio OpenCode

Considere somente modelos que atendam simultaneamente aos seguintes critérios:

1. estejam disponíveis diretamente no catálogo ou provider oficial do próprio OpenCode;
2. estejam identificados como gratuitos pelo OpenCode no ambiente atual;
3. não exijam API key de provider externo;
4. não exijam créditos pagos;
5. não gerem cobrança por token;
6. não dependam de assinatura externa;
7. não utilizem providers particulares configurados pelo usuário.

Não considere modelos provenientes diretamente de:

- OpenAI;
- Anthropic;
- Google;
- Amazon Bedrock;
- Azure OpenAI;
- GitHub Copilot;
- OpenRouter;
- Groq;
- Together AI;
- modelos locais;
- Ollama;
- LM Studio;
- qualquer outro provider externo.

Mesmo que um provider externo possua franquia, trial, créditos promocionais ou plano gratuito, ele não deverá ser considerado.

O requisito não é encontrar apenas modelos baratos ou incluídos em outra assinatura. O requisito é utilizar **somente modelos gratuitos fornecidos diretamente pelo OpenCode**.

### Validação obrigatória

Antes de recomendar um modelo:

- liste os modelos disponíveis pelo mecanismo oficial da instalação atual;
- identifique quais pertencem ao catálogo oficial do OpenCode;
- confirme quais estão marcados como gratuitos;
- utilize o identificador exato apresentado pelo OpenCode;
- não invente nomes ou IDs;
- não assuma que um modelo é gratuito apenas por estar disponível;
- não confunda modelo gratuito com trial, promoção, franquia ou créditos temporários.

Quando não for possível confirmar que o modelo é gratuito e fornecido diretamente pelo OpenCode, classifique-o como:

> Não elegível — gratuidade ou origem OpenCode não confirmada.

Esse modelo não poderá ser atribuído a nenhum agente.

Caso não existam modelos gratuitos do próprio OpenCode adequados para determinado papel, informe a limitação. Não substitua automaticamente por um modelo pago ou externo.

# Objetivo

Organizar os agentes locais do projeto em uma estrutura semelhante a:

- **Orchestrator**: recebe a solicitação, classifica a tarefa e coordena os agentes;
- **Explorer**: pesquisa o repositório e localiza arquivos, referências e padrões;
- **Developer**: implementa features, correções e refatorações;
- **Test Engineer**: cria e executa testes;
- **Architect**: avalia decisões arquiteturais e mudanças transversais;
- **Reviewer**: revisa o diff e identifica bugs e regressões;
- **Security Reviewer**: avalia autenticação, autorização, privacidade e exposição de dados;
- **Documentation Agent**: atualiza documentação técnica.

Essa lista não é obrigatória.

Primeiro, analise os agentes locais que já existem e determine:

- quais devem ser mantidos;
- quais devem ser alterados;
- quais estão duplicados;
- quais podem ser consolidados;
- quais podem ser removidos;
- quais agentes adicionais realmente agregariam valor;
- quais responsabilidades precisam ser separadas;
- se algum papel pode ser executado pelo mesmo agente para simplificar a estrutura.

Evite criar agentes em excesso.

# Etapa 1 — Identificar o escopo do projeto

Antes de qualquer análise:

1. identifique a raiz do repositório atual;
2. liste os arquivos locais relacionados ao OpenCode;
3. identifique a versão instalada do OpenCode;
4. confirme quais caminhos locais de configuração são suportados;
5. diferencie claramente arquivos locais e globais;
6. não altere nenhum arquivo.

Informe explicitamente a raiz do projeto identificada.

# Etapa 2 — Inspecionar a configuração local atual

Analise, quando existirem:

- `opencode.json`;
- `opencode.jsonc`;
- `.opencode/`;
- `.opencode/agents/`;
- `.opencode/commands/`;
- `AGENTS.md`;
- arquivos de instruções;
- documentação arquitetural;
- convenções do projeto;
- configurações locais relacionadas aos agentes.

Configurações globais podem ser consultadas somente para compreender heranças, comportamentos ou disponibilidade de modelos.

Não altere configurações globais.

Para cada agente local atual, informe:

1. nome;
2. caminho do arquivo;
3. modo, como `primary`, `subagent` ou equivalente;
4. modelo configurado;
5. origem do modelo;
6. ferramentas disponíveis;
7. permissões de leitura, escrita, edição e execução;
8. responsabilidade atual;
9. sobreposição com outros agentes;
10. riscos ou problemas;
11. recomendação de manutenção, alteração, fusão ou remoção.

Não altere arquivos nesta etapa.

# Etapa 3 — Descobrir os modelos gratuitos do OpenCode

Utilize os comandos ou mecanismos oficiais disponíveis na instalação atual para listar os modelos.

Não confie apenas em documentação antiga ou em nomes conhecidos.

Para cada modelo encontrado, registre:

- provider;
- identificador exato;
- nome apresentado;
- origem;
- indicação de gratuidade;
- eventual limite de uso;
- contexto disponível;
- suporte a ferramentas;
- suporte a raciocínio;
- qualidade esperada para programação;
- velocidade;
- limitações;
- tarefas mais adequadas.

Classifique cada modelo em uma destas categorias:

| Classificação | Definição |
|---|---|
| Elegível | Gratuito e fornecido diretamente pelo OpenCode |
| Não elegível: externo | Pertence a provider externo |
| Não elegível: pago | Possui cobrança ou exige créditos |
| Não elegível: promocional | Gratuidade temporária, trial ou promoção |
| Não elegível: não confirmado | Origem ou gratuidade não pôde ser comprovada |

Somente modelos classificados como **Elegível** poderão ser utilizados.

Caso o OpenCode apresente limites de requisição, contexto ou disponibilidade, registre essas limitações.

# Etapa 4 — Avaliar a capacidade dos modelos elegíveis

Avalie quais modelos gratuitos do OpenCode são mais adequados para:

- exploração do repositório;
- busca de arquivos e referências;
- implementação;
- correções;
- testes;
- documentação;
- planejamento;
- arquitetura;
- code review;
- segurança;
- tarefas com ferramentas;
- tarefas de contexto extenso.

Não escolha automaticamente o mesmo modelo para todos os papéis.

Entretanto, caso exista apenas um modelo gratuito elegível e suficientemente capaz, ele poderá ser reutilizado por múltiplos agentes, com instruções, ferramentas e permissões diferentes.

A separação entre agentes pode ser baseada em:

- responsabilidade;
- prompt;
- ferramentas;
- permissões;
- contexto;
- regras de acionamento;

e não obrigatoriamente em modelos diferentes.

# Etapa 5 — Criar a matriz de agentes e modelos

Apresente uma matriz como:

| Papel | Modelo gratuito principal | Alternativa gratuita | Elegibilidade confirmada | Motivo |
|---|---|---|---|---|
| Orchestrator | ... | ... | Sim/Não | ... |
| Explorer | ... | ... | Sim/Não | ... |
| Developer | ... | ... | Sim/Não | ... |
| Test Engineer | ... | ... | Sim/Não | ... |
| Architect | ... | ... | Sim/Não | ... |
| Reviewer | ... | ... | Sim/Não | ... |
| Security Reviewer | ... | ... | Sim/Não | ... |
| Documentation | ... | ... | Sim/Não | ... |

Não apresente modelos externos como alternativas.

Caso não exista um modelo gratuito adequado para um papel, registre:

> Sem modelo gratuito do OpenCode adequado para este papel.

Nesse caso, proponha uma destas soluções:

- manter o papel com capacidade limitada;
- exigir revisão humana;
- unir o papel a outro agente;
- não criar esse agente;
- interromper o fluxo e solicitar intervenção humana.

Não configure fallback automático para modelo pago ou externo.

# Etapa 6 — Propor o roteamento automático

Crie um fluxo semelhante a:

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

Todos os agentes devem utilizar apenas modelos elegíveis.

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
- feature envolvendo arquivos relacionados.

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

Caso os modelos gratuitos não sejam suficientemente confiáveis para uma decisão crítica, o agente deve parar e solicitar revisão humana.

# Etapa 7 — Definir regras objetivas

Proponha regras como:

- acionar `architect` quando houver migration;
- acionar `architect` quando mais de três módulos forem afetados;
- acionar `security-reviewer` quando houver autenticação ou autorização;
- acionar `security-reviewer` quando houver visibilidade por usuário, empresa, perfil, tenant, custodiante ou escriturador;
- acionar `reviewer` quando mais de dez arquivos forem modificados;
- acionar `test-engineer` sempre que uma regra de negócio for alterada;
- impedir `explorer` de editar arquivos;
- impedir `architect`, `reviewer` e `security-reviewer` de editar a implementação;
- permitir escrita somente aos agentes responsáveis pela implementação e documentação;
- exigir aprovação humana antes de migrations destrutivas;
- exigir aprovação humana antes de alterações irreversíveis;
- exigir aprovação humana quando o modelo gratuito não tiver capacidade adequada;
- nunca realizar fallback para modelo pago ou externo.

Adapte os limites à arquitetura real do projeto.

# Etapa 8 — Reduzir consumo de contexto

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
- respeitar eventuais limites dos modelos gratuitos;
- interromper a tarefa caso o limite gratuito seja atingido, em vez de migrar para um modelo pago.

# Etapa 9 — Propor a estrutura local final

Apresente:

1. estrutura final de agentes;
2. responsabilidades;
3. modelos gratuitos atribuídos;
4. permissões;
5. ferramentas;
6. critérios de acionamento;
7. fluxo de delegação;
8. arquivos que serão criados;
9. arquivos que serão alterados;
10. arquivos que serão consolidados ou removidos;
11. limitações dos modelos gratuitos;
12. pontos que exigirão revisão humana;
13. confirmação de que não haverá fallback pago;
14. confirmação de que não haverá alteração global.

Exemplo de estrutura:

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

# Etapa 10 — Criar comandos locais

Sugira comandos como:

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
- utilizar exclusivamente modelos gratuitos elegíveis;
- não interferir em outros projetos;
- não realizar fallback para provider externo;
- não realizar fallback para modelo pago;
- solicitar intervenção humana quando nenhum modelo gratuito for adequado.

# Etapa 11 — Segurança e versionamento

As configurações devem ser adequadas para versionamento no Git.

Não grave no repositório:

- tokens;
- API keys;
- credenciais;
- sessões;
- informações privadas;
- configurações globais copiadas;
- segredos de providers.

Caso o catálogo gratuito do OpenCode dependa de autenticação já existente, mantenha a autenticação fora do projeto.

No projeto, armazene somente:

- identificadores dos modelos;
- agentes;
- comandos;
- instruções;
- permissões;
- regras de roteamento.

Informe eventuais ajustes necessários no `.gitignore`.

# Etapa 12 — Aprovação obrigatória antes das alterações

Antes de editar qualquer arquivo:

1. apresente o diagnóstico;
2. apresente os modelos encontrados;
3. classifique a elegibilidade de cada modelo;
4. apresente a matriz de agentes;
5. apresente a arquitetura proposta;
6. apresente os caminhos que serão alterados;
7. confirme que todos estão dentro do projeto;
8. confirme que somente modelos gratuitos do OpenCode serão utilizados;
9. confirme que não haverá fallback pago ou externo;
10. confirme que nenhuma configuração global será modificada;
11. aguarde minha aprovação.

Somente depois da aprovação:

- crie ou altere arquivos locais;
- preserve configurações úteis existentes;
- não remova arquivos sem justificar;
- valide a sintaxe;
- valide os identificadores dos modelos;
- valide se todos os modelos continuam gratuitos;
- valide se os agentes podem ser carregados;
- valide se os comandos são reconhecidos;
- apresente o diff final;
- informe como testar;
- confirme que nenhum modelo externo ou pago foi configurado;
- confirme que nenhum arquivo global foi alterado.

# Critérios de qualidade

A solução deve:

- utilizar exclusivamente modelos gratuitos fornecidos diretamente pelo OpenCode;
- não utilizar trials ou promoções externas;
- não utilizar providers externos;
- não exigir API keys externas;
- não gerar cobrança por token;
- não possuir fallback para modelos pagos;
- funcionar na versão instalada;
- utilizar identificadores reais;
- ser totalmente local ao projeto;
- ser versionável no Git;
- não conter credenciais;
- minimizar leituras repetidas;
- separar responsabilidades;
- evitar agentes duplicados;
- exigir revisão humana em tarefas críticas;
- reconhecer as limitações dos modelos gratuitos;
- não afirmar que um modelo é gratuito sem confirmação;
- não alterar outros projetos.

# Formato da resposta inicial

Responda inicialmente com:

1. **Raiz do projeto identificada**
2. **Versão do OpenCode**
3. **Configuração local encontrada**
4. **Diagnóstico dos agentes atuais**
5. **Catálogo de modelos encontrado**
6. **Validação de origem e gratuidade**
7. **Modelos elegíveis**
8. **Modelos descartados e motivos**
9. **Matriz recomendada por papel**
10. **Arquitetura local proposta**
11. **Regras de roteamento**
12. **Limitações dos modelos gratuitos**
13. **Etapas que exigirão revisão humana**
14. **Estrutura de arquivos proposta**
15. **Plano de alterações**
16. **Confirmação de isolamento por projeto**
17. **Confirmação de ausência de modelos pagos ou externos**
18. **Aguardando aprovação**

Na confirmação final, declare explicitamente:

- quais arquivos locais pretende modificar;
- que nenhum arquivo global será alterado;
- que nenhuma credencial será adicionada ao repositório;
- que somente modelos gratuitos fornecidos diretamente pelo OpenCode serão considerados;
- que modelos externos, pagos, promocionais ou de gratuidade não confirmada serão descartados;
- que não será configurado fallback para modelos pagos;
- que a configuração valerá somente para o projeto atual.

Não realize alterações antes da minha aprovação.
