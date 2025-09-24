#Prompt 3 — Plano de Desenvolvimento Passo a Passo

Contexto: Com base nas Regras de Negócio (/docs/BusinessRules.md) e Arquitetura (/docs/Architecture.md), gere um plano prático de evolução do projeto.

Objetivo: Entregar um roteiro acionável (do setup ao deploy), priorizado e mensurável, apto a ser executado por um time (.NET/C#) em sprints.

Produto esperado (crie em /docs/DevelopmentPlan.md):
1) "Objetivos & Métricas" (OKRs/KPIs propostos, ex.: lead time, cobertura de testes, p95 de latência, erro %).
2) "Backlog Prioritário" (MoSCoW ou RICE):
   - Tabela: ID | Item | Motivo/Impacto | Esforço estimado (SP ou hm) | Dependências | Critérios de Aceite.
3) "Roadmap por Fases/Sprints"
   - Sequência passo a passo (Setup → Fundamentos → Funcionalidades → Hardening → Go-live).
   - Para cada passo: tarefas em checklist, donos sugeridos, artefatos (arquivos/projetos), riscos e mitigação.
4) "Padrões de Código & Qualidade"
   - Analyzers, formatadores, naming, nullable, warnings-as-errors, conventions, Sonar/coverage alvo.
5) "Estratégia de Testes"
   - Pirâmide (unit/integration/e2e), dados de teste, testes de contrato, mocks/fakes, cobertura alvo, testes de performance (k6 ou similar).
6) "Segurança & Compliance"
   - Threat model resumido (STRIDE), hardening (headers, secrets, permissões), SAST/DAST, dependabot.
7) "Observabilidade"
   - Logs estruturados, tracing distribuído, métricas de negócio/técnicas, dashboards iniciais, alertas (SLIs/SLOs).
8) "CI/CD"
   - Pipeline ideal (build, testes, quality gates, artefatos, deploy blue/green ou canary, rollback).
9) "Implantação & Operação"
   - Ambientes, migrações (EF), seed, feature flags, runbooks, SLOs, rotinas de manutenção.
10) "Riscos, Lacunas & Decisões"
    - Lista com prioridade e planos de ação.

Guidelines de geração:
- Referencie evidências do repositório (caminho + linhas) quando um item do plano deriva de um artefato específico.
- Forneça estimativas realistas por passo (escala curta), assinalando incertezas.
- Produza checklists em Markdown ([ ]), scripts de exemplo quando útil, e comandos para .NET CLI/docker se aplicável.
- Inclua uma seção “Definição de Pronto (DoD)” e “Critérios de Aceite” por épico/feature.
- Idioma: pt-BR, conciso e técnico.

Saída final:
- Arquivo único **/docs/DevelopmentPlan.md** com TOC, tabelas, checklists e links para os documentos gerados.
