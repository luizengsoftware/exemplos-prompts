#Prompt 1 — Regras de Negócio + Casos de Uso

Contexto: Você é uma IA analista de código com acesso somente-leitura ao workspace do projeto .NET/C#. Varra TODO o repositório aberto na IDE (incluindo *.cs, *.csproj, *.sln, *.md, *.json, *.yml, Dockerfile, pipelines, migrações EF, testes, scripts, etc.).

Objetivo: Extrair e documentar TODA a regra de negócio implementada e pretendida, com visão funcional completa (atores, casos de uso, fluxos, estados, invariantes, políticas, validações, exceções).

Escopo de análise (priorize em ordem):
1) Camadas de domínio (Entities/Aggregates/ValueObjects, Domain Services, Domain Events, Specifications, Validators).
2) Aplicação (Use Cases/Handlers, Services, Commands/Queries, DTOs, Validators).
3) Interface (Controllers, Endpoints, Filters, Middlewares).
4) Persistência (DbContext, Migrations, Configs, repos, transações).
5) Integrações externas (HTTP, mensageria, jobs, caches).
6) Documentos *.md (README, ADRs), appsettings*.json, Feature Flags, testes (unit/integration/e2e).

Produto esperado (crie em /docs/RegrasDeNegocio.md):
- "Resumo Executivo" (1–2 parágrafos).
- "Glossário" (Termo/Definição/Referência de código).
- "Atores e Permissões" (tabela: Ator | Responsabilidades | Permissões | Arquivos/linhas).
- "Casos de Uso" (para cada caso: Nome, Atores, Pré-condições, Fluxo Principal numerado, Fluxos Alternativos/Exceções, Pós-condições, Regra(s) de Negócio aplicada(s), Referências de código com caminho + linhas).
- "Regras de Negócio" (tabela: ID RB-### | Descrição clara | Onde é aplicada | Invariantes/Validações | Erros/Lançamentos | Arquivos/linhas).
- "Máquinas de Estado" para entidades-chave que mudam de status (diagrama + transições/guardas).
- "Políticas/SLAs/Restrições" (limites, idempotência, consistência, limites de taxa, etc.).
- "Cenários BDD (Gherkin)" para 5–10 fluxos críticos (Given/When/Then).

Diagramas:
- Para *Casos de Uso* e *Estados*, gere diagramas **Mermaid** (code blocks ```mermaid```).
  - Use Case: atores, casos, include/extend.
  - State: estados, eventos, transições, guardas.
- Inclua um "Índice de Diagramas" com links âncora.

Qualidade/checagens:
- Deduplicate e consolide regras repetidas; aponte conflitos e lacunas.
- Diferencie “regra implementada” vs “regra apenas documentada/pretendida” indicando a fonte (código, comentário, README, teste).
- Liste *assunções* explícitas quando inferir comportamento.
- Use português (pt-BR) técnico, referências de código no formato `caminho:linha-inicial–linha-final`.

Saída final:
- Arquivo único **/docs/RegrasDeNegocio.md** com sumário (TOC), tabelas, seções e diagramas Mermaid embutidos.
