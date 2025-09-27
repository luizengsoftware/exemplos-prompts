# Template para Projetos de Análise de AWS Parameter Store

## 📋 **Especificação Base**
Criar ferramenta Python para análise e exportação de parâmetros do AWS Systems Manager Parameter Store com múltiplos filtros e exportação CSV estruturada.

## 🎯 **Funcionalidades Essenciais**

### **1. Conectividade AWS**
- Suporte a credenciais via variáveis de ambiente (.env)
- Múltiplas regiões AWS configuráveis
- Validação de credenciais com diagnóstico detalhado
- Tratamento de erros de conectividade

### **2. Sistema de Filtros (3 Níveis)**
- **PARAMETER_PREFIXES**: Filtro por nome do parâmetro (ex: "apisettings")
- **FILTER_KEYS**: Filtro por chaves específicas dentro do valor JSON/Properties
- **CONTENT_FILTER**: Filtro LIKE por qualquer conteúdo no valor (case-insensitive)

### **3. Exportação CSV Inteligente**
- **Modo Filtrado**: Colunas dinâmicas baseadas em FILTER_KEYS
- **Modo Completo**: Coluna ParameterValue com valor completo
- Arquivo único consolidado com timestamp
- Coluna Region para identificar origem
- Escape de caracteres especiais (quebras de linha)

### **4. Parsing de Valores**
- Suporte a JSON estruturado
- Suporte a formato Properties/ENV (key=value)
- Extração individual de chaves para colunas separadas
- Tratamento de valores complexos e aninhados

## 🔧 **Arquitetura Técnica**

### **Estrutura de Classes**
```python
class ParameterStoreAnalyzer:
    - Configuração via .env
    - Métodos de parsing para cada tipo de filtro
    - Cliente SSM por região
    - Exportação CSV consolidada
    - Logging detalhado
```

### **Fluxo de Processamento**
1. Validação de credenciais AWS
2. Iteração por regiões configuradas
3. Aplicação sequencial de filtros:
   - Prefixos de parâmetros
   - Filtro de conteúdo geral
   - Filtro de chaves específicas
4. Consolidação de resultados
5. Exportação CSV única

### **Configuração (.env)**
```env
# Credenciais AWS
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_SESSION_TOKEN=
AWS_DEFAULT_REGION=us-east-1

# Regiões para análise
AWS_REGIONS=us-east-1,sa-east-1

# Filtros (todos opcionais)
PARAMETER_PREFIXES=apisettings
FILTER_KEYS=AWS_ACCESS_KEY_ID,DATABASE_URL
CONTENT_FILTER=prod.cloud.laqus.com.br
```

## 📊 **Formato de Saída CSV**

### **Colunas Base (sempre presentes)**
- Region
- ParameterName  
- Type
- LastModifiedDate
- Version

### **Colunas Dinâmicas (conforme filtros)**
- **Com FILTER_KEYS**: Uma coluna por chave filtrada
- **Sem FILTER_KEYS**: Coluna ParameterValue com conteúdo completo

## 🚀 **Casos de Uso Típicos**

### **Auditoria de Segurança**
```env
CONTENT_FILTER=password
# ou
CONTENT_FILTER=AKIA
```

### **Mapeamento de URLs por Ambiente**
```env
CONTENT_FILTER=prod.cloud.laqus.com.br
# ou
CONTENT_FILTER=.hml.cloud.laqus.com.br
```

### **Inventário de Conexões de Banco**
```env
CONTENT_FILTER=mysql
# ou
FILTER_KEYS=ConnectionString,DatabaseUrl
```

### **Análise de Configurações Específicas**
```env
PARAMETER_PREFIXES=apisettings
FILTER_KEYS=Redis,MongoDB,PostgreSQL
```

## 📝 **Logging e Debugging**
- Log detalhado de cada etapa do processamento
- Contagem de parâmetros por filtro aplicado
- Identificação de parâmetros encontrados por filtro
- Relatório final consolidado por região

## 🎯 **Prompt Direto para Projetos Similares**

"Preciso de um analisador Python para AWS Parameter Store com:
- Múltiplas regiões AWS
- 3 tipos de filtros: prefixo de nome, chaves específicas, conteúdo geral (LIKE)
- Exportação CSV consolidada com colunas dinâmicas
- Suporte a JSON e Properties nos valores
- Configuração via .env
- Logging detalhado
- Tratamento de erros AWS

Estrutura de saída CSV:
- Modo filtrado: colunas por chave específica
- Modo completo: coluna com valor integral
- Sempre incluir: Region, ParameterName, Type, LastModifiedDate, Version

Casos de uso: auditoria de segurança, mapeamento de URLs, inventário de bancos, análise de configurações."

## 🔄 **Padrões de Evolução**
- Adicionar novos tipos de filtros conforme necessidade
- Expandir formatos de parsing (YAML, XML, etc.)
- Implementar filtros combinados (AND/OR)
- Adicionar exportação para outros formatos (JSON, Excel)
- Implementar cache para otimização de performance
