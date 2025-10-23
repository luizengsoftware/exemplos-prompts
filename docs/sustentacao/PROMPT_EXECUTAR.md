# 🚀 PROMPT DE EXECUÇÃO - Análise de Sustentação

## Comando para Executar

```
Analise o arquivo CSV em relatorios/sustentacao_criados_60_dias_geral.csv seguindo as regras do PROMPT_ANALISE_TECH_LEAD.md.

Gere relatórios individuais em Markdown para cada módulo (DEPOSITÁRIA, ADMIN, SAAS, PORTAL, ESCRITURAÇÃO, OUTROS) seguindo este padrão:

## Estrutura de cada relatório:

# #N NOME_MÓDULO (X chamados - Y%)

## 📊 RESUMO
- Total de chamados, percentual, período
- Distribuição por Prioridade (🔴🟠🟡🟢 com %)
- Distribuição por Status (com emojis e %)
- Distribuição por Tipo (%)

## 🔥 TOP 3 PROBLEMAS MAIS COMUNS
Para cada problema:
- Ocorrências: X chamados
- Prioridade predominante
- Exemplos: IDs de chamados
- Causa raiz: análise técnica
- Impacto: consequências

## 💡 RECOMENDAÇÕES

### 🔴 CRÍTICO (Imediato - Sprint 23)
- Prioridade, Esforço (pontos), Impacto
- Ações específicas
- Resultado esperado

### 🟡 IMPORTANTE (Curto Prazo)
(mesmo formato)

### 🟢 MELHORIA CONTÍNUA (Médio Prazo)
(mesmo formato)

## 📈 MÉTRICAS DE SUCESSO
- Indicadores de acompanhamento
- Impacto estimado total

## 🎯 PRIORIZAÇÃO
- ROI estimado
- Ação imediata recomendada

---

Crie também um arquivo 00_indice.md com navegação entre todos os relatórios e resumo executivo consolidado.

Aplique as regras de agrupamento:
- DEPOSITÁRIA: Depositária, Depositária/Cadastro, Depositária/Arquivos
- ADMIN: Admin, Admin/Cadastro, Admin/Serv. Financ./BaaS BMP, Admin/RegistroTS
- SAAS: Monitor, Monitor/Dados de mercado, Mód. Contábil, Mód. Contábil/Integração ERP, Covenants
- PORTAL: Portal/Cadastro, Portal/SolicitacaoEmissao
- ESCRITURAÇÃO: Escrituração
- OUTROS: Outra aplicação, módulos não classificados

Processe os dados manualmente do CSV, sem usar Python ou scripts.
```

---

## ✅ Checklist de Qualidade

Ao executar, verifique se os relatórios contêm:

- [ ] Emojis de priorização (🔴🟡🟢)
- [ ] Percentuais em todas as distribuições
- [ ] IDs de chamados como exemplos
- [ ] Causa raiz técnica para cada problema
- [ ] Estimativas de esforço em pontos
- [ ] Impacto esperado quantificado
- [ ] Métricas de sucesso específicas
- [ ] ROI estimado para recomendações críticas

---

## 📁 Arquivos Esperados

```
relatorios/
├── 00_indice.md              # Índice geral
├── 01_depositaria.md         # Relatório Depositária
├── 02_admin.md               # Relatório Admin
├── 03_saas.md                # Relatório SaaS (Monitor + Contábil + Covenants)
├── 04_portal.md              # Relatório Portal
├── 05_escrituracao.md        # Relatório Escrituração
└── 06_outros.md              # Relatório Outros (se houver)
```

---

## 🎯 Dicas para Melhor Resultado

1. **Seja específico**: Mencione o arquivo CSV completo
2. **Peça análise manual**: Evite scripts automáticos
3. **Solicite exemplos**: IDs de chamados reais
4. **Exija métricas**: Percentuais e números absolutos
5. **Peça ROI**: Estimativas de impacto para priorização
