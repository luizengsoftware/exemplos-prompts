#Prompt 2 — Tecnologias & Arquitetura (C4)

Contexto: Projeto .NET/C#. Analise todos os arquivos relevantes: *.sln, *.csproj, Directory.Packages.props, packages.lock.json, global.json, Program.cs, Startup/Extensions, appsettings*.json, Dockerfile, docker-compose, Helm/K8s manifests, pipelines (GitHub Actions/Azure DevOps), *.md.

Objetivo: Mapear tecnologias, dependências e decisões arquiteturais e gerar diagramas no modelo **C4** (C1/C2/C3) com Mermaid (C4).

Produto esperado (crie em /docs/Architecture.md):
- "Stack & Dependências"
  - Tabelas: Linguagens & versões (.NET SDK/TargetFramework), Frameworks (ASP.NET Core, EF Core, etc.), Pacotes NuGet (Nome | Versão | Uso | Projeto), Banco(s) de dados, Mensageria, Cache, Observabilidade, Autenticação/Autorização, Cloud/Infra.
- "Configuração & Ambientes"
  - Variáveis sensíveis (somente nomes), Feature Flags, Connection Strings (somente chaves), perfis (Dev/QA/Prod).
- "Estilo Arquitetural"
  - DDD, Clean Architecture, CQRS/Event Sourcing (se houver), camadas, padrões adotados.
- "C4 Model" (Mermaid):
  - **C1 – System Context**: sistemas vizinhos, atores.
  - **C2 – Container**: APIs, BFFs, jobs, DBs, filas, gateways, caches, observabilidade.
  - **C3 – Component** (pelo menos para os containers mais críticos): controllers/handlers, services, repos, adaptadores, módulos.
  - Inclua notas de decisão (ADR inline): trade-offs, riscos, pontos quentes de performance/confiabilidade.
- "Topologia de Implantação"
  - Contêineres, imagens, portas, recursos, readiness/liveness, replicas/auto-scaling, storage, secrets (apenas referências), CD.
- "Pipeline CI/CD"
  - Build, testes, qualidade (analyzers, cobertura), artefatos, gates, deploy, rollback.

Instruções de diagrama:
- Gere **Mermaid** com sintaxe C4 (```mermaid → ```). Não renderize imagens; entregue o código pronto para renderização.
- Para cada nível (C1, C2, C3), forneça um bloco Mermaid separado, com legendas e relações rotuladas.

Qualidade/checagens:
- Vincule cada item de tecnologia a evidências: `projeto/arquivo:linha(s)` (por ex.: csproj que referencia pacote, Program.cs que registra middleware, yaml de pipeline).
- Marque **Tech Debt** detectada (versões antigas, pacotes abandonados, configurações inseguras).
- Marque **Decisões de Segurança** (auth, autorização, secrets, CORS, headers, TLS) e **Observabilidade** (logs, tracing, métricas).

Saída final:
- Arquivo único **/docs/Architecture.md** com TOC, tabelas e diagramas C4 em Mermaid.
