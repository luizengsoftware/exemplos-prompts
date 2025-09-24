#Prompt 2 — Tecnologias & Arquitetura (C4)

Contexto: Analise todos os diretórios e arquivos do repositório para identificar linguagens, frameworks, bibliotecas, serviços externos, bancos de dados, infraestrutura e pipelines. Considere qualquer stack (não apenas .NET).

Objetivo: Mapear tecnologias e arquiteturas adotadas, documentando e ilustrando em modelo C4.

Produto esperado (gerar em /docs/Architecture.md):
- Stack & Dependências
  - Linguagens e versões detectadas.
  - Frameworks e bibliotecas principais.
  - Serviços externos (bancos, mensageria, cache, autenticação, observabilidade, etc.).
- Configuração & Ambientes
  - Variáveis de ambiente, secrets (apenas nomes), feature flags.
- Estilo Arquitetural
  - Padrões adotados (DDD, Clean, Hexagonal, MVC, Monolito, Microservices, Serverless, etc.).
- Diagramas C4 em **Mermaid (C4)**
  - C1: System Context.
  - C2: Containers (APIs, jobs, DBs, caches, mensageria, etc.).
  - C3: Componentes críticos.
- Topologia de Deploy
  - Containers, imagens, serviços cloud, orquestração, scaling.
- Pipeline CI/CD
  - Build, testes, qualidade, deploy, rollback.

Qualidade:
- Sempre vincular tecnologia encontrada a evidências de arquivos/linhas.
- Marcar Tech Debt (versões antigas, pacotes obsoletos, configs inseguras).
- Marcar pontos de segurança, observabilidade e resiliência.
