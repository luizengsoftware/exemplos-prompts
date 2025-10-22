# 🔧 PROMPT GENÉRICO: ANÁLISE DE CHAMADOS COM EXTRAÇÃO HTML → CSV

## 🎯 OBJETIVO GERAL

Criar análises precisas de chamados do Jira exportados em HTML, usando extração para CSV como fonte de dados confiável.

---

## 📋 TEMPLATE DE ANÁLISE

### PASSO 1: DEFINIR TIPO DE ANÁLISE

**Escolha o agrupamento principal:**
- [ ] Por Responsável
- [ ] Por Módulo/Aplicação
- [ ] Por Prioridade
- [ ] Por Cliente
- [ ] Por Sprint
- [ ] Por Status
- [ ] Por Relator
- [ ] Por Tipo de Item

---

## 🔨 PASSO 2: EXTRAÇÃO DE DADOS

### Script PowerShell Base

```powershell
# ============================================
# CONFIGURAÇÃO
# ============================================
$htmlPath = "[CAMINHO_DO_ARQUIVO_HTML]"
$csvPath = "[CAMINHO_SAIDA]/chamados_extraidos.csv"

# ============================================
# EXTRAÇÃO DO HTML
# ============================================
$html = Get-Content $htmlPath -Raw

# Regex para extrair todos os campos relevantes
$pattern = @'
<tr\s+id="issuerow.*?
<td\s+class="issuekey">.*?>(MONIT-\d+)</a>.*?
<td\s+class="summary"><p>\s*(.*?)\s*</p>.*?
<td\s+class="priority">\s*(.*?)\s*</td>.*?
<td\s+class="customfield_10131">(.*?)</td>.*?
<td\s+class="customfield_10572">(.*?)</div>.*?
<td\s+class="status">.*?<span.*?>(.*?)</span>.*?
<td\s+class="assignee">(.*?)</td>.*?
<td\s+class="created">\s*(.*?)\s*</td>.*?
<td\s+class="reporter">(.*?)</td>.*?
<td\s+class="customfield_10010">(.*?)</td>
'@

$matches = [regex]::Matches($html, $pattern, [System.Text.RegularExpressions.RegexOptions]::Singleline)

# ============================================
# PROCESSAMENTO DOS DADOS
# ============================================
$chamados = @()
foreach($m in $matches) {
    # Extrair e limpar cada campo
    $monit = $m.Groups[1].Value
    $descricao = $m.Groups[2].Value -replace '<[^>]+>', '' -replace '\s+', ' ' -replace '^\s+|\s+$', '' -replace '&quot;', '"' -replace '&amp;', '&'
    $prioridade = $m.Groups[3].Value -replace '\s+', ' ' -replace '^\s+|\s+$', ''
    $cliente = $m.Groups[4].Value -replace '\s+', ' ' -replace '^\s+|\s+$', ''
    $modulo = $m.Groups[5].Value -replace '<[^>]+>', '' -replace '\s+', ' ' -replace '^\s+|\s+$', ''
    $status = $m.Groups[6].Value
    $responsavel = $m.Groups[7].Value -replace '\s+', ' ' -replace '^\s+|\s+$', '' -replace '<[^>]+>', ''
    $data = $m.Groups[8].Value
    $relator = $m.Groups[9].Value -replace '\s+', ' ' -replace '^\s+|\s+$', ''
    $sprint = $m.Groups[10].Value -replace '\s+', ' ' -replace '^\s+|\s+$', ''
    
    # Tratar campos vazios
    if($responsavel -eq '' -or $responsavel -match 'Não atribuído') { $responsavel = 'Não atribuído' }
    if($cliente -eq '') { $cliente = 'Não especificado' }
    if($modulo -eq '') { $modulo = 'Não especificado' }
    if($sprint -eq '') { $sprint = 'Sem Sprint' }
    
    # Criar objeto
    $chamados += [PSCustomObject]@{
        MONIT = $monit
        Descricao = $descricao
        Prioridade = $prioridade
        Cliente = $cliente
        Modulo = $modulo
        Status = $status
        Responsavel = $responsavel
        Data = $data
        Relator = $relator
        Sprint = $sprint
    }
}

# ============================================
# EXPORTAR PARA CSV
# ============================================
$chamados | Export-Csv $csvPath -NoTypeInformation -Encoding UTF8
Write-Output "✅ Exportados $($chamados.Count) chamados para CSV"
Write-Output "📁 Arquivo: $csvPath"
```

---

## 📊 PASSO 3: ANÁLISE E AGRUPAMENTO

### Script de Análise por Campo

```powershell
# ============================================
# ESCOLHA O CAMPO DE AGRUPAMENTO
# ============================================
$campoAgrupamento = "Responsavel"  # Alterar conforme necessário
# Opções: Responsavel, Modulo, Prioridade, Cliente, Sprint, Status, Relator

# ============================================
# FILTROS (OPCIONAL)
# ============================================
$statusFiltro = @("Pronto", "Em Produção", "Cancelado")  # Vazio = todos
$prioridadeFiltro = @()  # Ex: @("Highest", "High")
$sprintFiltro = @()  # Ex: @("Baikal 2025 - Sprint 21")

# ============================================
# APLICAR FILTROS
# ============================================
$chamadosFiltrados = $chamados
if($statusFiltro.Count -gt 0) {
    $chamadosFiltrados = $chamadosFiltrados | Where-Object { $_.Status -in $statusFiltro }
}
if($prioridadeFiltro.Count -gt 0) {
    $chamadosFiltrados = $chamadosFiltrados | Where-Object { $_.Prioridade -in $prioridadeFiltro }
}
if($sprintFiltro.Count -gt 0) {
    $chamadosFiltrados = $chamadosFiltrados | Where-Object { $_.Sprint -match ($sprintFiltro -join '|') }
}

Write-Output "📊 Total após filtros: $($chamadosFiltrados.Count) chamados"

# ============================================
# AGRUPAR E CONTAR
# ============================================
$grupos = @{}
foreach($chamado in $chamadosFiltrados) {
    $chave = $chamado.$campoAgrupamento
    
    if(-not $grupos.ContainsKey($chave)) {
        $grupos[$chave] = @{
            Pronto = 0
            'Em Produção' = 0
            Cancelado = 0
            Testando = 0
            'Em Desenvolvimento' = 0
            Outros = 0
            Total = 0
            Chamados = @()
        }
    }
    
    # Contar por status
    switch($chamado.Status) {
        "Pronto" { $grupos[$chave].Pronto++ }
        "Em Produção" { $grupos[$chave].'Em Produção'++ }
        "Cancelado" { $grupos[$chave].Cancelado++ }
        "Testando" { $grupos[$chave].Testando++ }
        "Em Desenvolvimento" { $grupos[$chave].'Em Desenvolvimento'++ }
        default { $grupos[$chave].Outros++ }
    }
    
    $grupos[$chave].Total++
    $grupos[$chave].Chamados += $chamado
}

# ============================================
# EXIBIR RESUMO
# ============================================
Write-Output "`n=== RESUMO POR $campoAgrupamento ===`n"
$grupos.GetEnumerator() | Sort-Object {$_.Value.Total} -Descending | ForEach-Object {
    $nome = $_.Key
    $stats = $_.Value
    $percentual = [math]::Round(($stats.Total / $chamadosFiltrados.Count) * 100, 1)
    
    Write-Output "$nome : $($stats.Total) chamados ($percentual%)"
    Write-Output "  ├─ Pronto: $($stats.Pronto)"
    Write-Output "  ├─ Em Produção: $($stats.'Em Produção')"
    Write-Output "  ├─ Cancelado: $($stats.Cancelado)"
    Write-Output "  ├─ Testando: $($stats.Testando)"
    Write-Output "  └─ Em Desenvolvimento: $($stats.'Em Desenvolvimento')"
    Write-Output ""
}

# ============================================
# EXPORTAR RESUMO
# ============================================
$resumo = $grupos.GetEnumerator() | Sort-Object {$_.Value.Total} -Descending | ForEach-Object {
    [PSCustomObject]@{
        $campoAgrupamento = $_.Key
        Total = $_.Value.Total
        Pronto = $_.Value.Pronto
        'Em Produção' = $_.Value.'Em Produção'
        Cancelado = $_.Value.Cancelado
        Testando = $_.Value.Testando
        'Em Desenvolvimento' = $_.Value.'Em Desenvolvimento'
        Percentual = [math]::Round(($_.Value.Total / $chamadosFiltrados.Count) * 100, 1)
    }
}

$resumoPath = $csvPath -replace '\.csv$', "_resumo_$campoAgrupamento.csv"
$resumo | Export-Csv $resumoPath -NoTypeInformation -Encoding UTF8
Write-Output "✅ Resumo exportado para: $resumoPath"
```

---

## 📝 PASSO 4: GERAR DOCUMENTO MARKDOWN

### Template de Estrutura

```markdown
# 📊 [TÍTULO DA ANÁLISE]

**Período:** [DATA_INICIO] a [DATA_FIM]
**Filtros Aplicados:** [DESCREVER FILTROS]
**Total Analisado:** [TOTAL] chamados

---

## 👥 RESUMO GERAL POR [CAMPO_AGRUPAMENTO]

| [Campo] | Total | Pronto | Em Produção | Cancelado | % |
|---------|-------|--------|-------------|-----------|---|
| ... | ... | ... | ... | ... | ... |

---

## 📋 DETALHAMENTO

### [GRUPO 1] ([TOTAL] chamados - [%])

#### Por Sprint / Por Status / Outro Subagrupamento

| MONIT | Descrição | [Campos Relevantes] | Status | Data |
|-------|-----------|---------------------|--------|------|
| ... | ... | ... | ... | ... |

---

## 📈 ANÁLISE E INSIGHTS

### ⚠️ Pontos de Atenção
1. [Insight 1]
2. [Insight 2]

### ✅ Pontos Positivos
1. [Insight 1]
2. [Insight 2]

---

## 📊 MÉTRICAS

### KPIs
- **[Métrica 1]:** [Valor]
- **[Métrica 2]:** [Valor]

### Metas Sugeridas
- **[Métrica 1]:** [Meta]
- **[Métrica 2]:** [Meta]

---

**Gerado em:** [DATA]
**Versão:** [VERSÃO]
```

---

## 🎨 CUSTOMIZAÇÕES POR TIPO DE ANÁLISE

### 1. Análise por Responsável

**Foco:**
- Produtividade individual
- Taxa de conclusão
- Especialização
- Distribuição de carga

**Métricas específicas:**
- Chamados por pessoa
- Taxa de conclusão por pessoa
- Concentração de trabalho
- Eficiência (Pronto / Total)

**Insights:**
- Identificar gargalos
- Avaliar distribuição
- Detectar especialistas
- Recomendar redistribuição

---

### 2. Análise por Módulo/Aplicação

**Foco:**
- Módulos críticos
- Complexidade por módulo
- Problemas recorrentes
- Necessidade de refatoração

**Métricas específicas:**
- Chamados por módulo
- Taxa de bugs vs tickets
- Módulos com mais cancelamentos
- Tempo médio por módulo

**Insights:**
- Identificar módulos problemáticos
- Priorizar refatoração
- Avaliar qualidade do código
- Recomendar melhorias arquiteturais

**Regras de agrupamento:**
```powershell
# Aplicar regras de agrupamento de módulos
function Get-ModuloAgrupado($modulo) {
    switch -Regex ($modulo) {
        '^Depositária' { return 'Depositária' }
        '^Admin' { return 'Admin' }
        '^Monitor' { return 'Monitor' }
        '^Portal' { return 'Portal' }
        '^Covenants' { return 'Covenants' }
        '^Escrituração' { return 'Escrituração' }
        default { return $modulo }
    }
}

# Aplicar agrupamento
foreach($chamado in $chamados) {
    $chamado.ModuloAgrupado = Get-ModuloAgrupado $chamado.Modulo
}
```

---

### 3. Análise por Prioridade

**Foco:**
- Cumprimento de SLA
- Tempo de resolução
- Priorização adequada
- Urgências vs importância

**Métricas específicas:**
- Chamados por prioridade
- Tempo médio de resolução por prioridade
- Taxa de escalação
- Prioridades alteradas

**Insights:**
- Avaliar processo de triagem
- Identificar falhas de priorização
- Recomendar ajustes de SLA
- Otimizar atendimento

---

### 4. Análise por Cliente

**Foco:**
- Clientes mais demandantes
- Satisfação por cliente
- Problemas recorrentes
- Oportunidades de melhoria

**Métricas específicas:**
- Chamados por cliente
- Taxa de resolução por cliente
- Tempo médio de atendimento
- Clientes com mais bugs

**Insights:**
- Identificar clientes críticos
- Priorizar melhorias
- Avaliar qualidade de entrega
- Recomendar ações proativas

---

### 5. Análise por Sprint

**Foco:**
- Capacidade da equipe
- Planejamento vs execução
- Tendências ao longo do tempo
- Previsibilidade

**Métricas específicas:**
- Chamados por sprint
- Taxa de conclusão por sprint
- Velocidade da equipe
- Carry over entre sprints

**Insights:**
- Avaliar capacidade
- Identificar sprints problemáticas
- Recomendar ajustes de planejamento
- Otimizar processo

---

## ✅ CHECKLIST DE QUALIDADE

### Extração de Dados
- [ ] Regex testado e validado
- [ ] Todos os campos necessários extraídos
- [ ] Limpeza de HTML aplicada
- [ ] Encoding UTF-8 configurado
- [ ] Campos vazios tratados
- [ ] CSV gerado com sucesso

### Análise
- [ ] Filtros aplicados corretamente
- [ ] Agrupamento funcionando
- [ ] Contagens validadas
- [ ] Percentuais corretos
- [ ] Totais conferidos

### Documento
- [ ] Estrutura completa
- [ ] Tabelas bem formatadas
- [ ] Dados precisos
- [ ] Insights relevantes
- [ ] Recomendações práticas
- [ ] Métricas claras

---

## 🚀 EXEMPLO DE USO COMPLETO

```powershell
# 1. Extrair dados do HTML
.\extrair_dados.ps1 -HtmlPath "relatorio.html" -CsvPath "dados.csv"

# 2. Analisar por responsável
.\analisar.ps1 -CsvPath "dados.csv" -Campo "Responsavel" -Status @("Pronto","Cancelado")

# 3. Gerar documento markdown
.\gerar_documento.ps1 -ResumoPath "dados_resumo_Responsavel.csv" -Tipo "Responsavel"
```

---

## 📚 REFERÊNCIAS

- **Arquivo HTML:** Exportação do Jira com filtro JQL
- **Campos do Jira:**
  - `issuekey`: MONIT-XXXX
  - `summary`: Descrição do chamado
  - `priority`: Highest, High, Medium, Low
  - `customfield_10131`: Cliente
  - `customfield_10572`: Aplicação/Módulo
  - `status`: Status atual
  - `assignee`: Responsável
  - `created`: Data de criação
  - `reporter`: Relator
  - `customfield_10010`: Sprint

---

**Versão:** 1.0  
**Data:** 22/10/2025  
**Uso:** Análises de chamados do Jira  
**Método:** Extração HTML → CSV → Análise → Markdown
