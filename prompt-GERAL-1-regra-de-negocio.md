#Prompt 1 — Regras de Negócio + Casos de Uso

Contexto: Você é uma IA analista de código com acesso somente-leitura ao workspace do projeto aberto. Varra TODO o repositório (código-fonte, configs, testes, docs *.md, scripts de build/deploy, etc.), independente da linguagem ou framework.

Objetivo: Extrair e documentar TODAS as regras de negócio implementadas e pretendidas, entregando uma visão funcional completa.

Produto esperado (gerar em /docs/BusinessRules.md):
- Resumo executivo (1–2 parágrafos).
- Glossário de termos de negócio (Termo | Definição | Referências no código).
- Atores e Permissões (tabela: Ator | Responsabilidades | Permissões | Arquivos/linhas).
- Casos de Uso (Nome | Atores | Pré-condições | Fluxo principal | Alternativos | Pós-condições | Regras aplicadas | Referências).
- Regras de Negócio (tabela RB-### | Descrição | Onde aplicada | Invariantes/Validações | Erros | Arquivos/linhas).
- Máquinas de Estado para entidades que mudam de status.
- Políticas/SLAs/Restrições.
- Cenários BDD (Given/When/Then) para fluxos críticos.

Diagramas:
- Casos de Uso e Estados em **PlantUML** (@startuml → @enduml).
- Indexar diagramas com links.

Qualidade:
- Diferenciar regras implementadas vs. apenas documentadas.
- Referenciar caminhos e linhas de código.
- Consolidar regras duplicadas, marcar lacunas.
- Idioma pt-BR, estilo técnico.
