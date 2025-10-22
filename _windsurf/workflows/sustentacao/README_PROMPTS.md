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
