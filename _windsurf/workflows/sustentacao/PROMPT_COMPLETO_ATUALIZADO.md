# 📋 PROMPT COMPLETO ATUALIZADO - ANÁLISE DE SUSTENTAÇÃO

## 🎯 OBJETIVO
Analisar chamados de sustentação dos últimos 60 dias, agrupando módulos relacionados para facilitar análise e tomada de decisão.

---

## 📥 ENTRADA

**Arquivo:** `SUSTENTAÇÃO - Criados - Últimos 60 dias - Geral.html`  
**Fonte:** Jira (exportação HTML)  
**Filtros aplicados:**
- Projetos: MONIT, SUST
- Período: Últimos 60 dias (created >= -60d)
- Tipos: Bug, Technical Debt, Ticket
- Ordenação: Prioridade DESC, Data criação ASC

---

## 🔧 REGRAS DE AGRUPAMENTO DE MÓDULOS

### DEPOSITÁRIA (agrupa)
- Depositária
- Depositária/Cadastro
- Depositária/Arquivos
- **Justificativa:** Todos fazem parte do mesmo sistema de gestão de instrumentos financeiros

### ADMIN (agrupa)
- Admin (generalizado)
- Admin/Cadastro
- Admin/Serv. Financ./BaaS BMP
- Admin/RegistroTS
- **Justificativa:** Todos fazem parte do sistema administrativo/backoffice

### MONITOR (agrupa)
- Monitor
- Monitor/Dados de mercado
- **Justificativa:** Sistema de monitoramento e dados de mercado são interdependentes

### MÓDULO CONTÁBIL (agrupa)
- Mód. Contábil
- Mód. Contábil/Integração ERP
- **Justificativa:** Módulo contábil e suas integrações são parte do mesmo domínio

### PORTAL (agrupa)
- Portal/Cadastro
- Portal/SolicitacaoEmissao
- **Justificativa:** Funcionalidades do portal do cliente

### ESCRITURAÇÃO (mantém)
- Escrituração
- **Justificativa:** Módulo específico e independente

### COVENANTS (mantém)
- Covenants
- **Justificativa:** Módulo específico de gestão de covenants

### OUTROS (agrupa)
- Outra aplicação
- Módulos não classificados ou vazios
- **Justificativa:** Chamados sem classificação clara

---

## 📊 ANÁLISES REQUERIDAS

### 1. RANKING POR MÓDULO AGRUPADO
- Listar todos os módulos agrupados
- Quantidade de chamados por módulo
- Percentual do total
- Ordenar do maior para o menor

### 2. ANÁLISE DETALHADA TOP 5 MÓDULOS

Para cada um dos 5 módulos com mais chamados:

#### A. Estatísticas Gerais
- Total de chamados
- Percentual do total geral
- Distribuição por prioridade (Highest, High, Medium, Low)
- Distribuição por tipo (Bug, Ticket, Technical Debt)
- Distribuição por status (Pronto, Em Produção, Entrada, Cancelado, etc.)

#### B. TOP 3 Problemas Identificados
Para cada problema:
- **Descrição:** O que está acontecendo
- **Causa Raiz:** Por que está acontecendo (análise técnica)
- **Chamados Relacionados:** IDs dos chamados (mínimo 3, máximo 8) com descrição completa um em cada linha
- **Impacto:** Crítico/Alto/Médio/Baixo + justificativa
- **Padrão Identificado:** Se há recorrência ou padrão

#### C. Padrões Críticos
- Problemas recorrentes (chamados clonados, mesma descrição)
- Integrações com falhas
- Módulos com alta taxa de bugs
- Funcionalidades problemáticas

#### D. Recomendações Priorizadas
Tabela com:
- Prioridade (🔴 CRÍTICO, 🟡 ALTO, 🟢 MÉDIO)
- Ação específica e mensurável
- Prazo em sprints
- Responsável sugerido (role, não pessoa)

### 3. ANÁLISE TRANSVERSAL

Identificar problemas que afetam múltiplos módulos:

#### Para cada problema transversal:
- **Nome do Problema**
- **Módulos Afetados**
- **Quantidade de Ocorrências**
- **Criticidade** (🔴 CRÍTICA, 🟡 ALTA, 🟢 MÉDIA)
- **Chamados Relacionados** (IDs)
- **Causa Raiz**
- **Recomendação** com prazo

**Exemplos de problemas transversais:**
- Integração entre sistemas (IMF ↔ Monitor)
- Gestão de cadastros (Admin ↔ Portal ↔ Depositária)
- Arquivos e conciliações (Depositária ↔ Escrituração)

### 4. ESTATÍSTICAS GERAIS

#### Por Prioridade
- Highest: X (Y%)
- High: X (Y%)
- Medium: X (Y%)
- Low: X (Y%)

#### Por Tipo
- Bug: X (Y%)
- Ticket: X (Y%)
- Technical Debt: X (Y%)

#### Por Status
- Pronto: X (Y%)
- Em Produção: X (Y%)
- Entrada de chamado: X (Y%)
- Cancelado: X (Y%)
- Desenvolvimento: X (Y%)
- Testar/Testando: X (Y%)
- Outros: X (Y%)

#### Taxa de Resolução
- **Resolvidos:** (Pronto + Em Produção) / Total
- **Em Andamento:** (Desenvolvimento + Testar) / Total
- **Aguardando:** (Entrada + Backlog) / Total
- **Cancelados:** Cancelados / Total

#### Clientes Mais Impactados
- Top 5 clientes por quantidade de chamados
- Incluir "Todos" se aplicável

### 5. GRÁFICOS E VISUALIZAÇÕES

Gerar em formato Mermaid:

#### Obrigatórios:
1. **Pizza:** Distribuição por módulo agrupado
2. **Pareto:** Análise 80/20 (concentração em poucos módulos)
3. **Timeline:** Evolução por sprint
4. **Funil:** Status dos chamados
5. **Heatmap:** Módulo vs Prioridade
6. **Fluxo:** Problemas transversais

#### Opcionais:
7. Matriz de priorização (Impacto vs Esforço)
8. Indicadores de saúde do sistema
9. Roadmap visual de melhorias

### 6. ROADMAP DE MELHORIAS

Dividir em 3 fases:

#### FASE 1 - ESTABILIZAÇÃO (Sprints 23-24 / 4 semanas)
- Objetivo: Reduzir criticidade e estabilizar operação
- Ações prioritárias (🔴 CRÍTICO)
- Meta mensurável

#### FASE 2 - OTIMIZAÇÃO (Sprints 25-26 / 4-8 semanas)
- Objetivo: Melhorar eficiência e qualidade
- Ações importantes (🟡 ALTO)
- Meta mensurável

#### FASE 3 - TRANSFORMAÇÃO (Sprints 27-30 / 8-16 semanas)
- Objetivo: Arquitetura sustentável
- Ações estratégicas (🟢 MÉDIO)
- Meta mensurável

### 7. INDICADORES-CHAVE (KPIs)

#### Saúde do Sistema
Tabela com:
- Indicador
- Valor Atual
- Meta
- Status (🔴 Crítico / 🟡 Atenção / 🟢 OK)

**Indicadores obrigatórios:**
- Taxa de Bugs (meta: <25%)
- Chamados Highest (meta: <30%)
- Taxa de Resolução (meta: >60%)
- Concentração Top 3 (meta: <60%)
- Taxa de Cancelamento (meta: <10%)

#### Eficiência Operacional
- Chamados/Sprint (média)
- Módulo mais crítico
- Retrabalho (chamados clonados)
- Dívida Técnica (quantidade)
- Crescimento (% entre sprints)

### 8. RECOMENDAÇÕES EXECUTIVAS

#### Ações Imediatas (Esta Sprint)
- Máximo 5 ações
- Foco em estabilização
- Impacto rápido

#### Ações de Curto Prazo (1-2 meses)
- Máximo 5 ações
- Foco em qualidade
- Impacto médio

#### Ações Estratégicas (3-6 meses)
- Máximo 5 ações
- Foco em arquitetura
- Impacto longo prazo

#### ROI Estimado
Tabela com:
- Iniciativa
- Investimento (sprints)
- Economia Anual (horas ou %)
- ROI (%)

---

## 📤 SAÍDA - ESTRUTURA DOS RELATÓRIOS

### Arquivo 1: README.md
- Índice de navegação
- Principais números
- TOP 5 ações prioritárias
- Roadmap resumido
- Contatos e próximos passos

### Arquivo 2: SUMARIO_EXECUTIVO.md
**Público:** C-Level, Gestores, Product Owners  
**Tempo de leitura:** 5 minutos

**Conteúdo:**
- Principais achados (4-5 bullets)
- TOP 5 problemas críticos
- Impacto financeiro e ROI
- Plano de ação (3 fases)
- Métricas de sucesso
- Riscos e mitigações
- Próximos passos

### Arquivo 3: RELATORIO_ANALISE_COMPLETA_60_DIAS.md
**Público:** Tech Leads, Arquitetos, Desenvolvedores  
**Tempo de leitura:** 20 minutos

**Conteúdo:**
- Ranking completo por módulo agrupado
- Análise detalhada TOP 5 módulos
- Análise transversal
- Estatísticas gerais completas
- Gráficos Mermaid
- Roadmap de melhorias (6 meses)
- KPIs e indicadores
- Recomendações executivas

### Arquivo 4: GRAFICOS_DETALHADOS.md
**Público:** Apresentações, Dashboards  
**Tempo de leitura:** 10 minutos

**Conteúdo:**
- 12 gráficos Mermaid interativos
- Cada gráfico com título e análise
- Formatação pronta para apresentação

### Arquivo 5: LISTA_DETALHADA_CHAMADOS.md
**Público:** Referência, Auditoria  
**Tempo de leitura:** Consulta

**Conteúdo:**
- Lista completa organizada por módulo agrupado
- Tabelas com ID, título, status, sprint
- Resumo estatístico
- Chamados clonados (retrabalho)
- Chamados críticos não resolvidos

---

## 🎨 FORMATAÇÃO E ESTILO

### Markdown
- Usar emojis para destacar seções (📊 📈 🔥 🎯 💡 🚨 ⚠️ ✅ 🔴 🟡 🟢)
- Títulos com # ## ### ####
- Tabelas bem formatadas
- Listas com bullets
- Código em blocos ```mermaid```
- Negrito para **ênfase**
- Itálico para _observações_

### Linguagem
- Português brasileiro
- Tom profissional mas direto
- Evitar jargões desnecessários
- Usar termos técnicos quando apropriado
- Números sempre com contexto

### Gráficos Mermaid
- Usar cores consistentes:
  - 🔴 Crítico: #ff0000 ou #ff6b6b
  - 🟡 Alto: #ffa500 ou #ffeb3b
  - 🟢 Médio: #4caf50 ou #4ecdc4
  - Neutro: #2196f3 ou #45b7d1
- Títulos descritivos
- Legendas quando necessário
- Tamanho adequado para leitura

---

## ✅ CHECKLIST DE QUALIDADE

Antes de finalizar, verificar:

- [ ] Todos os módulos foram agrupados corretamente
- [ ] Percentuais somam 100% (considerando múltiplos módulos por chamado)
- [ ] TOP 5 módulos têm análise completa
- [ ] Problemas transversais identificados
- [ ] Recomendações têm prazo e responsável
- [ ] ROI estimado para principais iniciativas
- [ ] Gráficos Mermaid renderizam corretamente
- [ ] Tabelas estão bem formatadas
- [ ] Números conferem entre seções
- [ ] Linguagem profissional e clara
- [ ] Todos os 5 arquivos foram gerados
- [ ] README tem índice completo

---

## 🔄 PROCESSO DE EXECUÇÃO

**⚠️ IMPORTANTE: A análise deve ser feita DIRETAMENTE pela IA ao ler o arquivo HTML.**
**NÃO usar scripts Python, Node.js ou outras ferramentas externas.**
**A IA deve processar os dados em memória e gerar os relatórios Markdown diretamente.**

1. **Ler e Processar HTML do Jira**
   - Ler o arquivo HTML completo usando a ferramenta `read_file`
   - Extrair TODOS os 144 chamados diretamente do HTML
   - Identificar campos: Chave, Tipo, Resumo, Prioridade, Cliente, Módulo(s), Status, Data Criação, Sprint
   - **ATENÇÃO:** Um chamado pode ter MÚLTIPLOS módulos (campo `customfield_10572`)

2. **Aplicar Agrupamento de Módulos (em memória)**
   - Para cada módulo encontrado, aplicar as regras de agrupamento definidas
   - Contar ocorrências por módulo agrupado
   - **IMPORTANTE:** Se um chamado tem múltiplos módulos, ele conta para CADA módulo agrupado
   - Exemplo: MONIT-8477 tem "Admin (generalizado)" + "Escrituração" → conta 1 para ADMIN e 1 para ESCRITURAÇÃO

3. **Gerar Estatísticas (em memória)**
   - Calcular totais por módulo agrupado
   - Calcular percentuais (soma pode ser > 100% devido a múltiplos módulos)
   - Identificar TOP 5 módulos
   - Gerar estatísticas por prioridade, tipo, status, cliente
   - Identificar padrões e problemas recorrentes

4. **Análise Detalhada (em memória)**
   - Para cada TOP 5 módulos: analisar chamados relacionados
   - Identificar TOP 3 problemas por módulo
   - Encontrar problemas transversais (que afetam múltiplos módulos)
   - Analisar chamados clonados (CLONE no título)

5. **Gerar Gráficos Mermaid**
   - Criar visualizações em formato Mermaid
   - Validar sintaxe dos gráficos

6. **Escrever Relatórios Markdown**
   - Gerar os 5 arquivos Markdown usando `write_to_file`
   - Aplicar formatação consistente
   - Incluir todos os gráficos e tabelas
   - Revisar qualidade final

7. **Validação Final**
   - Conferir se todos os 144 chamados foram processados
   - Verificar se números batem entre seções
   - Confirmar que todos os 5 arquivos foram criados

---

## 📝 EXEMPLO DE USO

```
Analise o arquivo "SUSTENTAÇÃO - Criados - Últimos 60 dias - Geral.html" 
seguindo este prompt completo. Agrupe os módulos conforme as regras 
definidas e gere os 5 relatórios em Markdown com gráficos Mermaid.
```

---

## 🔧 MANUTENÇÃO DO PROMPT

**Versão:** 2.1  
**Data:** 22/10/2025  
**Alterações:**
- ✅ Adicionadas regras de agrupamento de módulos
- ✅ Unificado Depositária + Depositária/Cadastro + Depositária/Arquivos
- ✅ Unificado Admin + Admin/Cadastro + Admin/Serv.Fin. + Admin/RegistroTS
- ✅ Unificado Monitor + Monitor/Dados de mercado
- ✅ Unificado Mód. Contábil + Mód. Contábil/Integração ERP
- ✅ Unificado Portal/Cadastro + Portal/SolicitacaoEmissao
- ✅ **IMPORTANTE:** Especificado que análise deve ser feita DIRETAMENTE pela IA (sem scripts Python/Node.js)
- ✅ Clarificado que processamento deve ser em memória ao ler o HTML
- ✅ Adicionado exemplo de múltiplos módulos por chamado

**Próximas Melhorias:**
- Adicionar análise de tendências (crescimento/decrescimento)
- Incluir previsão de chamados para próximas sprints
- Adicionar análise de SLA e tempo de resolução
- Incluir análise de desenvolvedores mais alocados

---

## 📚 PROMPTS AUXILIARES

### Para Análises Específicas com Extração HTML → CSV

**Quando usar extração HTML → CSV:**
- ✅ Precisão é crítica (contagens, percentuais)
- ✅ Análise envolve múltiplos agrupamentos
- ✅ Necessário validar dados antes de gerar documento
- ✅ Análise complexa com filtros e cruzamentos

**Prompts disponíveis:**

1. **`PROMPT_ANALISE_POR_RESPONSAVEL.md`**
   - Análise detalhada de chamados resolvidos por responsável
   - Método: Extração HTML → CSV → Análise
   - Inclui: Produtividade, taxa de conclusão, especialização
   - Scripts PowerShell incluídos

2. **`PROMPT_ANALISE_GENERICA.md`**
   - Template genérico para qualquer tipo de análise
   - Adaptável para: Responsável, Módulo, Prioridade, Cliente, Sprint
   - Inclui: Scripts PowerShell completos, estrutura markdown, checklist de qualidade
   - Exemplos de customização por tipo de análise

**Como usar:**
```
1. Escolher o tipo de análise desejada
2. Usar script PowerShell para extrair dados do HTML para CSV
3. Validar contagens e totais
4. Gerar documento markdown seguindo estrutura do prompt
5. Incluir insights e recomendações acionáveis
```

---

**FIM DO PROMPT**
