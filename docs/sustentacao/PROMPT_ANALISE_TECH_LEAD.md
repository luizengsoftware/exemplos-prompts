# 🧠 PROMPT — Análise Sustentação 60 dias (3 Visões)

**Contexto:**  
Sou **Tech Lead** de uma Squad de Sustentação que também atua com inovação.  
O time tem 6 desenvolvedores, com **Sprints quinzenais** (2 semanas) e **10 pontos por Sprint**.  
Quero analisar o arquivo `sustentacao_criados_60_dias_geral.csv` com base nos **últimos 60 dias**, sob **3 visões estratégicas**.  

A análise deve aplicar as **regras de agrupamento de módulos** descritas abaixo e apresentar as **saídas estruturadas, com texto, ícones e percentuais**.

**⚠️ IMPORTANTE:** Analise o CSV diretamente, **SEM utilizar Python ou scripts**. Leia o arquivo CSV e processe os dados manualmente para gerar o relatório.

---

## ⚙️ REGRAS DE AGRUPAMENTO DE MÓDULOS

### **DEPOSITÁRIA**
- Depositária  
- Depositária/Cadastro  
- Depositária/Arquivos  
> Justificativa: Sistema de gestão de instrumentos financeiros  

### **ADMIN**
- Admin  
- Admin/Cadastro  
- Admin/Serv. Financ./BaaS BMP  
- Admin/RegistroTS  
> Justificativa: Backoffice e serviços administrativos  

### **SAAS**
- Monitor  
- Monitor/Dados de mercado  
- Mód. Contábil  
- Mód. Contábil/Integração ERP  
- Covenants  
> Justificativa: Interdependência entre monitoramento e dados de mercado, domínio contábil e integrações ERP, gestão de covenants  

### **PORTAL**
- Portal/Cadastro  
- Portal/SolicitacaoEmissao  
> Justificativa: Funcionalidades do portal do cliente  

### **ESCRITURAÇÃO**
- Escrituração  
> Justificativa: Módulo independente  

### **OUTROS**
- Outra aplicação  
- Módulos não classificados ou vazios  
> Justificativa: Sem classificação clara  

---

**Contexto:**
- Período: [últimos X dias / data específica]
- Filtros aplicados: [prioridade, status, tipo, etc.]

**Entregáveis:**

1. **TOP 3 Problemas Mais Comuns**
   - Agrupe por similaridade técnica/funcional por módulo (utilize as regras de agrupamento)
   - Inclua: % de ocorrência, prioridade predominante, exemplos (IDs)
   - Análise de causa raiz para cada grupo

2. **Distribuições Estatísticas**
   - Por prioridade (Highest, High, Medium, Low)
   - Por tipo (Bug, Ticket, Technical Debt)
   - Por status (Pronto, Em Desenvolvimento, Entrada, Cancelado, etc.)

3. **Análise Adicional**
   - Identifique padrões críticos (ex: integrações, bloqueios)
   - Destaque chamados com flag "Impedimento"
   - Agrupe problemas secundários relevantes

4. **Recomendações Priorizadas**
   - Crítico (curto prazo): problemas que bloqueiam operação
   - Importante (médio prazo): melhorias de performance/qualidade
   - Melhoria contínua: prevenção e documentação
   - Inclua impacto estimado para cada recomendação

**Formato de saída:**
- Arquivo Markdown estruturado
- Use emojis para priorização visual (🔴🟡🟢)
- Inclua percentuais e números absolutos
- Seja objetivo e técnico

**Observações:**
- Considere apenas chamados [abertos/todos/resolvidos]
- Ignore chamados duplicados/cancelados [se aplicável]
- Foque em ações práticas e mensuráveis