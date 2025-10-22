# 📋 PROMPT: ANÁLISE DE CHAMADOS RESOLVIDOS POR RESPONSÁVEL

## 🎯 OBJETIVO

Gerar documento markdown com análise detalhada de chamados resolvidos (Pronto, Em Produção, Cancelado) agrupados por responsável e por sprint, incluindo estatísticas e insights de produtividade.

---

## 📝 INSTRUÇÕES

### 1️⃣ EXTRAÇÃO DE DADOS DO HTML

**Usar PowerShell para extrair dados diretamente do HTML exportado do Jira:**

```powershell
# Passo 1: Extrair todos os chamados resolvidos para CSV
$html = Get-Content "[CAMINHO_DO_ARQUIVO_HTML]" -Raw

$pattern = '<tr id="issuerow.*?<td class="issuekey">.*?>(MONIT-\d+)</a>.*?<td class="summary"><p>\s*(.*?)\s*</p>.*?<td class="customfield_10572">(.*?)</div>.*?<td class="status">.*?<span.*?>(Pronto|Em Produção|Cancelado)</span>.*?<td class="assignee">(.*?)</td>.*?<td class="created">\s*(.*?)\s*</td>.*?<td class="reporter">(.*?)</td>.*?<td class="customfield_10010">(.*?)</td>'

$matches = [regex]::Matches($html, $pattern, [System.Text.RegularExpressions.RegexOptions]::Singleline)

$chamados = @()
foreach($m in $matches) {
    $monit = $m.Groups[1].Value
    $desc = $m.Groups[2].Value -replace '<[^>]+>', '' -replace '\s+', ' ' -replace '^\s+|\s+$', ''
    $modulo = $m.Groups[3].Value -replace '<[^>]+>', '' -replace '\s+', ' ' -replace '^\s+|\s+$', ''
    $status = $m.Groups[4].Value
    $assignee = $m.Groups[5].Value -replace '\s+', ' ' -replace '^\s+|\s+$', '' -replace '<[^>]+>', ''
    if($assignee -eq '') { $assignee = 'Não atribuído' }
    $data = $m.Groups[6].Value
    $reporter = $m.Groups[7].Value -replace '\s+', ' ' -replace '^\s+|\s+$', ''
    $sprint = $m.Groups[8].Value -replace '\s+', ' ' -replace '^\s+|\s+$', ''
    
    $chamados += [PSCustomObject]@{
        MONIT=$monit
        Descricao=$desc
        Modulo=$modulo
        Status=$status
        Responsavel=$assignee
        Data=$data
        Relator=$reporter
        Sprint=$sprint
    }
}

$chamados | Export-Csv "[CAMINHO_SAIDA]/chamados_resolvidos.csv" -NoTypeInformation -Encoding UTF8
Write-Output "Exportados $($chamados.Count) chamados para CSV"
```

**Passo 2: Gerar contagem por responsável**

```powershell
$responsaveis = @{}
foreach($m in $matches) {
    $status = $m.Groups[4].Value
    $assignee = $m.Groups[5].Value -replace '\s+', ' ' -replace '^\s+|\s+$', '' -replace '<[^>]+>', ''
    if($assignee -eq '') { $assignee = 'Não atribuído' }
    
    if(-not $responsaveis.ContainsKey($assignee)) {
        $responsaveis[$assignee] = @{Pronto=0; 'Em Produção'=0; Cancelado=0; Total=0}
    }
    $responsaveis[$assignee][$status]++
    $responsaveis[$assignee]['Total']++
}

Write-Output "=== CONTAGEM DE CHAMADOS RESOLVIDOS POR RESPONSÁVEL ===`n"
$responsaveis.GetEnumerator() | Sort-Object {$_.Value.Total} -Descending | ForEach-Object {
    Write-Output "$($_.Key): Total=$($_.Value.Total) (Pronto=$($_.Value.Pronto), Em Produção=$($_.Value.'Em Produção'), Cancelado=$($_.Value.Cancelado))"
}
```

---

### 2️⃣ ESTRUTURA DO DOCUMENTO MARKDOWN

**Seções obrigatórias:**

1. **Cabeçalho**
   - Título: "📊 CHAMADOS RESOLVIDOS POR RESPONSÁVEL E SPRINT"
   - Período analisado
   - Status considerados
   - Total de resolvidos

2. **Resumo Geral por Responsável**
   - Tabela com: Responsável, Pronto, Em Produção, Cancelado, Total, %
   - Ordenado por Total (decrescente)

3. **Detalhamento por Responsável** (Top 5)
   - Seção para cada responsável com:
     - Nome e total de chamados (%)
     - Subsections por Sprint
     - Tabela com: MONIT, Descrição, Módulo, Status, Data, Relator

4. **Outros Responsáveis** (se > 5)
   - Seção agrupada para responsáveis com menos chamados

5. **Não Atribuído**
   - Seção separada para chamados sem responsável

6. **Resumo por Sprint**
   - Tabela com: Sprint, Pronto, Em Produção, Cancelado, Total, Responsáveis Ativos

7. **Análise de Produtividade**
   - **Por Responsável:**
     - Top 3 com análise detalhada
     - Taxa de conclusão
     - Foco/especialização
     - Perfil do desenvolvedor
   
   - **Por Sprint:**
     - Análise de cada sprint
     - Identificação de picos e quedas
     - Taxa de conclusão por sprint

8. **Insights e Recomendações**
   - **Pontos de Atenção (⚠️):**
     - Concentração de trabalho
     - Alta taxa de cancelamento
     - Chamados não atribuídos
     - Sprints problemáticas
   
   - **Pontos Positivos (✅):**
     - Distribuição equilibrada
     - Alta taxa de conclusão
     - Especialização eficiente
     - Desenvolvedores com eficiência máxima

9. **Métricas de Acompanhamento**
   - **KPIs Atuais:**
     - Taxa de Resolução
     - Taxa de Conclusão
     - Taxa de Cancelamento
     - Concentração de Trabalho
     - Média por Sprint
     - Responsáveis Ativos
   
   - **Metas Sugeridas:**
     - Valores alvo para cada KPI

10. **Rodapé**
    - Data de geração
    - Versão do documento
    - Nota sobre atualização

---

### 3️⃣ REGRAS DE ANÁLISE

**Filtros:**
- ✅ Incluir apenas: Status = "Pronto" OU "Em Produção" OU "Cancelado"
- ❌ Excluir: "Em Desenvolvimento", "Testando", "Aguardando", etc.

**Agrupamentos:**
- **Por Responsável:** Campo "assignee" do HTML
- **Por Sprint:** Campo "customfield_10010" do HTML
- **Por Status:** Campo "status" do HTML

**Cálculos:**
- **Taxa de Conclusão:** (Pronto / Total) × 100
- **Taxa de Cancelamento:** (Cancelado / Total) × 100
- **Concentração:** (Maior Total Individual / Total Geral) × 100
- **Média por Sprint:** Total / Número de Sprints

**Ordenação:**
- Responsáveis: Por Total (decrescente)
- Sprints: Por ordem cronológica
- Chamados: Por Data (crescente)

---

### 4️⃣ CRITÉRIOS DE QUALIDADE

**Precisão:**
- ✅ Usar extração direta do HTML (não inferir dados)
- ✅ Validar contagens com comando PowerShell
- ✅ Conferir totais e percentuais

**Completude:**
- ✅ Incluir TODOS os responsáveis (mesmo com 1 chamado)
- ✅ Incluir TODAS as sprints mencionadas
- ✅ Não omitir chamados não atribuídos

**Clareza:**
- ✅ Usar emojis para destacar seções
- ✅ Usar tabelas markdown bem formatadas
- ✅ Incluir contexto nas análises (não apenas números)

**Acionabilidade:**
- ✅ Insights devem ter recomendações práticas
- ✅ Identificar padrões e tendências
- ✅ Destacar riscos e oportunidades

---

## 🔄 ADAPTAÇÃO PARA OUTRAS ANÁLISES

### Análise por Módulo/Aplicação

**Modificações necessárias:**

1. **Extração PowerShell:**
```powershell
# Agrupar por Módulo ao invés de Responsável
$modulos = @{}
foreach($chamado in $chamados) {
    $modulo = $chamado.Modulo
    if($modulo -eq '') { $modulo = 'Não especificado' }
    
    if(-not $modulos.ContainsKey($modulo)) {
        $modulos[$modulo] = @{Pronto=0; 'Em Produção'=0; Cancelado=0; Total=0}
    }
    $modulos[$modulo][$chamado.Status]++
    $modulos[$modulo]['Total']++
}
```

2. **Estrutura do Documento:**
   - Título: "📊 CHAMADOS RESOLVIDOS POR MÓDULO/APLICAÇÃO"
   - Seção principal: Por Módulo (ao invés de Por Responsável)
   - Análise: Focar em módulos críticos, complexidade, etc.

3. **Regras de Agrupamento de Módulos:**
   - Aplicar regras definidas em `PROMPT_COMPLETO_ATUALIZADO.md`
   - Exemplo: Agrupar "Depositária", "Depositária/Cadastro", "Depositária/Arquivos"

### Análise por Prioridade

**Modificações necessárias:**

1. **Extração PowerShell:**
```powershell
# Adicionar campo Prioridade no regex
$pattern = '...<td class="priority">\s*(.*?)\s*</td>...'
```

2. **Estrutura do Documento:**
   - Título: "📊 CHAMADOS RESOLVIDOS POR PRIORIDADE"
   - Análise: Tempo de resolução por prioridade, SLA, etc.

### Análise por Cliente

**Modificações necessárias:**

1. **Extração PowerShell:**
```powershell
# Adicionar campo Cliente no regex
$pattern = '...<td class="customfield_10131">(.*?)</td>...'
```

2. **Estrutura do Documento:**
   - Título: "📊 CHAMADOS RESOLVIDOS POR CLIENTE"
   - Análise: Clientes mais demandantes, satisfação, etc.

---

## 📌 EXEMPLO DE USO

```markdown
# Comando para executar análise completa

1. Extrair dados do HTML para CSV
2. Gerar contagem por responsável
3. Ler CSV e criar documento markdown seguindo estrutura
4. Validar totais e percentuais
5. Gerar insights e recomendações
```

---

## ⚠️ PONTOS DE ATENÇÃO

1. **Encoding UTF-8:** Sempre usar UTF-8 para evitar problemas com acentuação
2. **Limpeza de HTML:** Remover tags HTML dos campos extraídos
3. **Espaços em branco:** Normalizar espaços múltiplos e trim
4. **Campos vazios:** Tratar como "Não atribuído" ou "Não especificado"
5. **Regex:** Testar regex antes de processar arquivo completo
6. **Validação:** Sempre validar contagem total com comando separado

---

## 🎯 RESULTADO ESPERADO

- ✅ Documento markdown completo e bem formatado
- ✅ Dados 100% precisos (extraídos diretamente do HTML)
- ✅ Análises contextualizadas e acionáveis
- ✅ Insights relevantes para tomada de decisão
- ✅ Métricas e KPIs claros
- ✅ Recomendações práticas

---

**Versão:** 1.0  
**Data:** 22/10/2025  
**Autor:** Análise de Sustentação  
**Aplicável a:** Análises por Responsável, Módulo, Prioridade, Cliente, Sprint
