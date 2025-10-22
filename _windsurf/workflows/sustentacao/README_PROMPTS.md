# 📚 GUIA DE PROMPTS - ANÁLISE DE SUSTENTAÇÃO

## 🎯 Visão Geral

Este diretório contém prompts especializados para análise de chamados de sustentação exportados do Jira. Cada prompt foi projetado para um tipo específico de análise, garantindo precisão e consistência nos resultados.

---

## 📋 ÍNDICE DE PROMPTS

### 1. PROMPT_COMPLETO_ATUALIZADO.md
**Tipo:** Análise Geral Completa  
**Método:** Leitura direta do HTML pela IA  
**Tempo:** ~30-60 minutos  

**Quando usar:**
- Análise completa dos últimos 60 dias
- Visão executiva para gestores
- Identificação de problemas críticos
- Planejamento de roadmap

**Gera:**
- ✅ README.md
- ✅ SUMARIO_EXECUTIVO.md
- ✅ RELATORIO_ANALISE_COMPLETA_60_DIAS.md
- ✅ GRAFICOS_DETALHADOS.md
- ✅ LISTA_DETALHADA_CHAMADOS.md

**Características:**
- Agrupamento de módulos relacionados
- TOP 5 módulos com análise detalhada
- Problemas transversais
- Gráficos Mermaid
- Roadmap de melhorias (6 meses)
- KPIs e indicadores

---

### 2. PROMPT_ANALISE_POR_RESPONSAVEL.md
**Tipo:** Análise de Produtividade  
**Método:** Extração HTML → CSV → Análise  
**Tempo:** ~15-20 minutos  

**Quando usar:**
- Avaliar produtividade da equipe
- Identificar gargalos e dependências
- Analisar distribuição de carga
- Avaliar especialização dos desenvolvedores

**Gera:**
- ✅ CHAMADOS_RESOLVIDOS_POR_RESPONSAVEL.md
- ✅ chamados_resolvidos.csv (intermediário)

**Características:**
- Extração precisa via PowerShell
- Agrupamento por responsável e sprint
- Taxa de conclusão por pessoa
- Análise de especialização
- Insights de produtividade
- Recomendações de redistribuição

**Filtros:**
- Status: Pronto, Em Produção, Cancelado
- Período: Últimos 60 dias

**Scripts incluídos:**
```powershell
# Extração de dados
# Contagem por responsável
# Validação de totais
```

---

### 3. PROMPT_ANALISE_GENERICA.md
**Tipo:** Template Reutilizável  
**Método:** Extração HTML → CSV → Análise  
**Tempo:** ~10-30 minutos (depende da customização)  

**Quando usar:**
- Criar análises customizadas
- Análise por módulo/aplicação
- Análise por prioridade
- Análise por cliente
- Análise por sprint
- Qualquer outro agrupamento

**Gera:**
- ✅ Documento markdown customizado
- ✅ CSV com dados extraídos
- ✅ CSV com resumo por agrupamento

**Características:**
- Template 100% adaptável
- Scripts PowerShell genéricos
- Estrutura markdown flexível
- Checklist de qualidade
- Exemplos de customização

**Adaptável para:**
- Por Responsável
- Por Módulo/Aplicação
- Por Prioridade
- Por Cliente
- Por Sprint
- Por Status
- Por Relator
- Por Tipo de Item

**Scripts incluídos:**
```powershell
# Extração base (todos os campos)
# Análise por campo customizável
# Filtros configuráveis
# Exportação de resumos
```

---

## 🔄 FLUXO DE TRABALHO

### Análise Completa (Mensal/Trimestral)
```
1. Exportar HTML do Jira
2. Usar PROMPT_COMPLETO_ATUALIZADO.md
3. Gerar 5 documentos markdown
4. Apresentar para stakeholders
```

### Análise de Produtividade (Semanal/Quinzenal)
```
1. Exportar HTML do Jira
2. Usar PROMPT_ANALISE_POR_RESPONSAVEL.md
3. Executar script PowerShell de extração
4. Validar contagens
5. Gerar documento markdown
6. Compartilhar com tech leads
```

### Análise Customizada (Sob Demanda)
```
1. Exportar HTML do Jira
2. Usar PROMPT_ANALISE_GENERICA.md
3. Customizar campo de agrupamento
4. Configurar filtros
5. Executar scripts PowerShell
6. Gerar documento markdown
```

---

## 🎨 QUANDO USAR CADA MÉTODO

### Método 1: Leitura Direta pela IA
**Prompts:** PROMPT_COMPLETO_ATUALIZADO.md

**Vantagens:**
- ✅ Mais rápido (sem scripts externos)
- ✅ Análise contextual rica
- ✅ Insights automáticos
- ✅ Gráficos Mermaid integrados

**Desvantagens:**
- ❌ Menos preciso em contagens complexas
- ❌ Difícil validar dados intermediários
- ❌ Limitado a capacidade da IA

**Usar quando:**
- Análise exploratória
- Visão geral é suficiente
- Tempo é crítico
- Insights qualitativos são mais importantes

---

### Método 2: Extração HTML → CSV
**Prompts:** PROMPT_ANALISE_POR_RESPONSAVEL.md, PROMPT_ANALISE_GENERICA.md

**Vantagens:**
- ✅ Precisão máxima (100%)
- ✅ Dados validáveis
- ✅ Reutilizável (CSV)
- ✅ Auditável

**Desvantagens:**
- ❌ Requer execução de scripts
- ❌ Mais etapas no processo
- ❌ Necessita PowerShell

**Usar quando:**
- Precisão é crítica
- Dados serão auditados
- Múltiplas análises do mesmo dataset
- Contagens complexas (múltiplos agrupamentos)

---

## 📊 COMPARAÇÃO DE MÉTODOS

| Critério | Leitura Direta | Extração CSV |
|----------|----------------|--------------|
| **Precisão** | 95-98% | 100% |
| **Velocidade** | ⚡⚡⚡ Rápido | ⚡⚡ Médio |
| **Validação** | Difícil | Fácil |
| **Reutilização** | Baixa | Alta |
| **Complexidade** | Baixa | Média |
| **Auditoria** | Difícil | Fácil |
| **Insights** | Rico | Básico |
| **Gráficos** | Automático | Manual |

---

## 🛠️ REQUISITOS TÉCNICOS

### Para Todos os Prompts
- ✅ Acesso ao Jira
- ✅ Permissão para exportar HTML
- ✅ Editor de markdown
- ✅ IA com capacidade de leitura de arquivos

### Para Prompts com Extração CSV
- ✅ PowerShell 5.1+ (Windows)
- ✅ Conhecimento básico de PowerShell
- ✅ Editor de CSV (Excel, LibreOffice)

---

## 📝 EXEMPLOS DE USO

### Exemplo 1: Análise Mensal Completa
```markdown
Prompt: PROMPT_COMPLETO_ATUALIZADO.md

Comando:
"Analise o arquivo 'SUSTENTAÇÃO - Criados - Últimos 60 dias - Geral.html' 
seguindo o PROMPT_COMPLETO_ATUALIZADO.md. Agrupe os módulos conforme as 
regras definidas e gere os 5 relatórios em Markdown com gráficos Mermaid."

Resultado:
- 5 documentos markdown
- 12 gráficos Mermaid
- Roadmap de 6 meses
- KPIs e recomendações

Exemplo de Saída - TOP 3 Problemas DEPOSITÁRIA:

🔴 1. Gestão de Eventos Financeiros (8 chamados)
Causa Raiz: Engine complexa sem documentação, cálculos descentralizados
Exemplos:
- MONIT-8530: Tela de eventos financeiros não está funcionando
- MONIT-8541: IFs não refletem a agenda de eventos de Juros - Energisa
- MONIT-8560: CLONE - IF não reflete a agenda de eventos de Juros
- MONIT-8564: CLONE - Eventos não constam na tela de Eventos Financeiros
- MONIT-8516: Exclusão de eventos de juros triplicados - LNC002502407
- MONIT-8517: Excluir eventos duplicados - LNC002301233
Impacto: 🔴 CRÍTICO - Afeta cálculos financeiros e relatórios

🔴 2. Arquivos de Conciliação (12 chamados)
Causa Raiz: Processo não idempotente, transações longas, falta de retry
Exemplos:
- MONIT-8654: Disponibilizar arquivos de conciliação para escrituradores
- MONIT-8661: Melhorar rotina de geração de arquivos de conciliação
- MONIT-8662: Disponibilizar arquivos de conciliação - 08/10
- MONIT-8575: Conciliações geradas indevidamente aos Participantes
- MONIT-8666: CLONE - Conciliações geradas indevidamente
- MONIT-8640: Arquivos não foram gerados ao Participante Daycoval
Impacto: 🔴 CRÍTICO - Impede conciliação diária

🟡 3. Edição de Operações IMF (6 chamados)
Causa Raiz: Validações restritivas, locks não liberados
Exemplos:
- MONIT-8572: Não conseguimos editar instrumentos financeiros - IMF
- MONIT-8625: ERRO PARA EDITAR E CORRIGIR OPERAÇÕES - IMF
- MONIT-8706: CLONE - ERRO PARA EDITAR E CORRIGIR OPERAÇÕES
- MONIT-8532: Erro ao Transferir titularidade - LCF002500001
Impacto: 🟡 ALTO - Bloqueia correções urgentes
```

### Exemplo 2: Análise de Produtividade Semanal
```markdown
Prompt: PROMPT_ANALISE_POR_RESPONSAVEL.md

Comando:
"Analise o arquivo HTML exportado do Jira seguindo o 
PROMPT_ANALISE_POR_RESPONSAVEL.md. Use os scripts PowerShell para 
extrair dados para CSV e gere o documento de análise de produtividade."

Resultado:
- 1 documento markdown
- 1 CSV com dados brutos
- Análise por responsável e sprint
- Insights de produtividade

Exemplo de Saída - TOP 3 Desenvolvedores:

🥇 1. Joelson Cerqueira - 18 chamados resolvidos
Sprint 21: 12 chamados (66.7%)
Sprint 20: 6 chamados (33.3%)
Especialização: DEPOSITÁRIA (83.3%)
Exemplos:
- MONIT-8654: Disponibilizar arquivos de conciliação ✅ Pronto
- MONIT-8661: Melhorar rotina de geração de arquivos ✅ Pronto
- MONIT-8667: Realizar baixa de conciliações em lote ✅ Pronto
Taxa de Conclusão: 94.4%

🥈 2. Luiz Silva - 14 chamados resolvidos
Sprint 21: 8 chamados (57.1%)
Sprint 22: 6 chamados (42.9%)
Especialização: ADMIN (50%), MONITOR (35.7%)
Exemplos:
- MONIT-8675: Criar usuário conforme padrão Iveco/CNH ✅ Pronto
- MONIT-8678: Habilitar Módulo Contábil [Airela] ✅ Pronto
- MONIT-8695: Alterar emissor da operação LNC002503124 🔄 Desenvolvimento
Taxa de Conclusão: 85.7%

🥉 3. Deividy Ferreira - 8 chamados resolvidos
Sprint 19: 4 chamados (50%)
Sprint 20: 4 chamados (50%)
Especialização: ESCRITURAÇÃO (62.5%), DEPOSITÁRIA (37.5%)
Exemplos:
- MONIT-8542: Cadastro de Instituição Financeira (FIDD) ✅ Pronto
- MONIT-8524: Popular banco de dados para Cota de Fundo ✅ Pronto
Taxa de Conclusão: 100%
```

### Exemplo 3: Análise Customizada por Módulo
```markdown
Prompt: PROMPT_ANALISE_GENERICA.md

Comando:
"Use o PROMPT_ANALISE_GENERICA.md para criar uma análise de chamados 
agrupados por Módulo/Aplicação. Configure o campo de agrupamento como 
'Modulo' e aplique as regras de agrupamento do PROMPT_COMPLETO_ATUALIZADO.md."

Resultado:
- 1 documento markdown customizado
- 1 CSV com dados por módulo
- 1 CSV com resumo por módulo
- Análise de módulos críticos

Exemplo de Saída - Ranking por Módulo:

🔴 1. DEPOSITÁRIA - 82 chamados (56.9%)
Prioridade Highest: 24 (29.3%)
Bugs: 22 (26.8%)
Taxa de Resolução: 56.1%
Status: CRÍTICO - Requer ação imediata

Principais Problemas:
- Eventos Financeiros: 8 chamados
  * MONIT-8530, 8541, 8560, 8564, 8516, 8517, 8506, 8529
- Conciliações: 12 chamados
  * MONIT-8654, 8661, 8662, 8575, 8666, 8640, 8653, 8643
- Edição de IFs: 6 chamados
  * MONIT-8572, 8625, 8706, 8532, 8562, 8563

🟡 2. ADMIN - 13 chamados (9.0%)
Prioridade Highest: 4 (30.8%)
Bugs: 3 (23.1%)
Taxa de Resolução: 76.9%
Status: ALTO - Monitoramento necessário

Principais Problemas:
- Cadastros: 3 chamados
  * MONIT-8507, 8603, 8566
- BaaS BMP: 5 chamados
  * MONIT-8652, 8683, 8702, 8559, 8561

🟡 3. MONITOR - 13 chamados (9.0%)
Prioridade Highest: 6 (46.2%)
Bugs: 5 (38.5%)
Taxa de Resolução: 61.5%
Status: ALTO - Problemas de relatórios

Principais Problemas:
- Relatório de Posição: 3 chamados
  * MONIT-8649, 8664, 8677
- Integração IMF: 2 chamados
  * MONIT-8614, 8615
- Dados de Mercado: 3 chamados
  * MONIT-8494, 8680, 8648
```

---

## 🔧 MANUTENÇÃO E VERSIONAMENTO

### Estrutura de Versões
- **Major (X.0.0):** Mudanças estruturais significativas
- **Minor (0.X.0):** Novas funcionalidades ou seções
- **Patch (0.0.X):** Correções e ajustes

### Histórico de Versões

**PROMPT_COMPLETO_ATUALIZADO.md**
- v2.1 (22/10/2025): Adicionado prompts auxiliares
- v2.0 (22/10/2025): Adicionadas regras de agrupamento
- v1.0 (Inicial): Versão base

**PROMPT_ANALISE_POR_RESPONSAVEL.md**
- v1.0 (22/10/2025): Versão inicial

**PROMPT_ANALISE_GENERICA.md**
- v1.0 (22/10/2025): Versão inicial

---

## 📞 SUPORTE E CONTRIBUIÇÕES

### Dúvidas Frequentes

**Q: Qual prompt devo usar para análise rápida?**
A: Use PROMPT_COMPLETO_ATUALIZADO.md com leitura direta pela IA.

**Q: Preciso de precisão máxima nas contagens. Qual usar?**
A: Use PROMPT_ANALISE_POR_RESPONSAVEL.md ou PROMPT_ANALISE_GENERICA.md com extração CSV.

**Q: Como criar uma análise por prioridade?**
A: Use PROMPT_ANALISE_GENERICA.md e customize o campo de agrupamento para "Prioridade".

**Q: Os scripts PowerShell funcionam no Linux/Mac?**
A: Não. Para Linux/Mac, adapte os scripts para Bash ou use PowerShell Core.

**Q: Posso combinar múltiplos prompts?**
A: Sim! Use PROMPT_COMPLETO_ATUALIZADO.md para visão geral e PROMPT_ANALISE_POR_RESPONSAVEL.md para drill-down.

### Melhorias Futuras

**Planejado:**
- [ ] Prompt para análise de SLA e tempo de resolução
- [ ] Prompt para análise de tendências temporais
- [ ] Prompt para previsão de chamados
- [ ] Scripts para Linux/Mac (Bash)
- [ ] Integração com APIs do Jira
- [ ] Dashboard automático

**Sugestões:**
Abra uma issue ou envie um pull request com suas sugestões de melhorias.

---

## 📚 RECURSOS ADICIONAIS

### Documentação Relacionada
- Regras de agrupamento de módulos
- Glossário de termos técnicos
- Guia de interpretação de KPIs
- Exemplos de roadmaps

### Ferramentas Úteis
- [Mermaid Live Editor](https://mermaid.live) - Testar gráficos Mermaid
- [Markdown Preview](https://markdownlivepreview.com) - Visualizar markdown
- [PowerShell ISE](https://docs.microsoft.com/powershell) - Editor PowerShell

---

**Última atualização:** 22/10/2025  
**Versão do guia:** 1.0  
**Mantido por:** Equipe de Sustentação
