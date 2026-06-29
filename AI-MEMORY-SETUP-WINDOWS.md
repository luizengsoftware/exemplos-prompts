# ai-memory: Instalação Local Completa no Windows (Servidor + Cliente)

> Guia para Windows nativo com Docker Desktop.  
> Última atualização: 2026-06-28

---

## Visão Geral

O ai-memory fornece memória de longo prazo para agentes de IA (Kiro, Claude Code, Cursor, etc.).  
A arquitetura é simples:

```
┌─────────────────────────────────────────────┐
│  Agente (Kiro, Cursor, Claude Code, etc.)   │
│  ↕ MCP (JSON-RPC via HTTP)                  │
├─────────────────────────────────────────────┤
│  Servidor ai-memory (Docker Desktop)        │
│  - SQLite + FTS5 (wiki/pages)               │
│  - Bind: 127.0.0.1:49374                    │
│  - Volume: ai-memory-data:/data             │
├─────────────────────────────────────────────┤
│  Cliente ai-memory (binary nativo Windows)  │
│  - CLI para search, status, bootstrap, etc. │
│  - Path: %LOCALAPPDATA%\ai-memory\          │
└─────────────────────────────────────────────┘
```

---

## Pré-requisitos

| Requisito | Verificação (PowerShell) |
|-----------|--------------------------|
| Docker Desktop instalado e rodando | `docker --version` |
| PowerShell 5.1+ | `$PSVersionTable.PSVersion` |
| Acesso à internet | Para baixar imagem e binário |

> **Docker Desktop:** Baixe em https://www.docker.com/products/docker-desktop/ e garanta que está rodando (ícone na bandeja do sistema).

---

## Parte 1: Servidor (Docker Desktop)

### 1.1. Subir o container (modo zero-LLM)

Abra o **PowerShell** e execute:

```powershell
docker run -d --name ai-memory `
    --restart unless-stopped `
    -p 127.0.0.1:49374:49374 `
    -v ai-memory-data:/data `
    akitaonrails/ai-memory:latest
```

> Modo zero-LLM: busca, handoffs e wiki funcionam sem chave de API.  
> Consolidação automática e embeddings ficam desabilitados.

### 1.2. Com LLM para consolidação e embeddings (OpenAI)

```powershell
docker run -d --name ai-memory `
    --restart unless-stopped `
    -p 127.0.0.1:49374:49374 `
    -v ai-memory-data:/data `
    -e AI_MEMORY_LLM_PROVIDER=openai `
    -e OPENAI_API_KEY=sk-sua-chave-openai-aqui `
    -e AI_MEMORY_EMBEDDING_PROVIDER=openai `
    akitaonrails/ai-memory:latest
```

### 1.3. Com Anthropic (LLM) + OpenAI (embeddings)

```powershell
docker run -d --name ai-memory `
    --restart unless-stopped `
    -p 127.0.0.1:49374:49374 `
    -v ai-memory-data:/data `
    -e AI_MEMORY_LLM_PROVIDER=anthropic `
    -e ANTHROPIC_API_KEY=sk-ant-sua-chave-aqui `
    -e AI_MEMORY_EMBEDDING_PROVIDER=openai `
    -e OPENAI_API_KEY=sk-sua-chave-openai-aqui `
    akitaonrails/ai-memory:latest
```

### 1.4. Verificar que o servidor subiu

```powershell
# Container rodando e healthy
docker ps | Select-String "ai-memory"

# Testar endpoint MCP
Invoke-RestMethod -Uri "http://127.0.0.1:49374/mcp" `
    -Method POST `
    -ContentType "application/json" `
    -Headers @{ "Accept" = "application/json, text/event-stream" } `
    -Body '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2024-11-05","capabilities":{},"clientInfo":{"name":"test","version":"1.0"}}}'
```

Resposta esperada: JSON com `"serverInfo":{"name":"ai-memory","version":"1.4.x"}`.

### 1.5. Gerenciamento do container

```powershell
# Parar
docker stop ai-memory

# Iniciar
docker start ai-memory

# Logs
docker logs ai-memory --tail 50

# Remover container (dados preservados no volume)
docker rm -f ai-memory

# Atualizar imagem
docker pull akitaonrails/ai-memory:latest
docker rm -f ai-memory
# Re-executar o docker run do passo 1.1 ou 1.2
```

### 1.6. Backup dos dados

```powershell
docker run --rm -v ai-memory-data:/data -v ${PWD}:/backup `
    alpine tar czf /backup/ai-memory-backup-$(Get-Date -Format "yyyyMMdd").tar.gz /data
```

---

## Parte 2: Cliente (Binary Nativo Windows)

### 2.1. Instalar o binário

```powershell
# Criar diretório
New-Item -ItemType Directory -Force -Path "$env:LOCALAPPDATA\ai-memory"

# Baixar release
$release = "https://github.com/akitaonrails/ai-memory/releases/latest/download/ai-memory-windows-x86_64.zip"
Invoke-WebRequest -Uri $release -OutFile "$env:TEMP\ai-memory.zip"

# Extrair
Expand-Archive "$env:TEMP\ai-memory.zip" -DestinationPath "$env:LOCALAPPDATA\ai-memory" -Force

# Limpar
Remove-Item "$env:TEMP\ai-memory.zip"
```

### 2.2. Adicionar ao PATH (permanente)

```powershell
# Adicionar ao PATH do usuário
$currentPath = [Environment]::GetEnvironmentVariable("Path", "User")
if ($currentPath -notlike "*ai-memory*") {
    [Environment]::SetEnvironmentVariable("Path", "$env:LOCALAPPDATA\ai-memory;$currentPath", "User")
}

# Recarregar PATH na sessão atual
$env:Path = "$env:LOCALAPPDATA\ai-memory;$env:Path"

# Verificar
ai-memory --version
```

> **Importante:** Feche e reabra o PowerShell (ou terminal do Kiro/Cursor) para o PATH valer em todos os processos.

### 2.3. Testar conexão com o servidor local

```powershell
ai-memory status
```

Saída esperada:
```
ai-memory 1.4.1 (server)
  server:       http://127.0.0.1:49374
  pages:        0 (all versions: 0)
  sessions:     0
  observations: 0
  providers:
    llm:       openai/gpt-4o-mini
    embedding: openai/text-embedding-3-small
```

### 2.4. Configurar o projeto/workspace (`.ai-memory.toml`)

Cada repositório precisa de um arquivo `.ai-memory.toml` na raiz para identificar o projeto dentro do ai-memory. Sem ele, as observações podem cair no projeto genérico `scratch`.

```powershell
# Na raiz do repositório
@"
workspace = "default"
project = "afiliados"
"@ | Set-Content -Path ".ai-memory.toml" -Encoding UTF8
```

Ou crie manualmente o arquivo `.ai-memory.toml` na raiz do projeto com o conteúdo:

```toml
workspace = "default"
project = "afiliados"
```

| Campo | Descrição |
|-------|-----------|
| `workspace` | Agrupamento lógico de projetos. Use `"default"` se tem apenas um workspace |
| `project` | Nome único do projeto. Deve ser descritivo e sem espaços (use hífen ou underscore) |

> **Importante:** adicione `.ai-memory.toml` ao controle de versão (git) para que todos os devs da equipe compartilhem o mesmo identificador de projeto.

Exemplo para múltiplos projetos na mesma máquina:

```
C:\Users\dev\repos\Afiliados\.ai-memory.toml   → project = "afiliados"
C:\Users\dev\repos\Landing\.ai-memory.toml     → project = "landing"
C:\Users\dev\repos\OutroApp\.ai-memory.toml    → workspace = "outro-cliente", project = "app"
```

---

## Parte 3: Configuração do Agente (Kiro)

### 3.1. Configuração MCP

Arquivo: `.kiro\settings\mcp.json` (workspace) ou `%USERPROFILE%\.kiro\settings\mcp.json` (global)

```json
{
  "mcpServers": {
    "ai-memory": {
      "url": "http://127.0.0.1:49374/mcp",
      "disabled": false,
      "autoApprove": ["*"]
    }
  }
}
```

> O Kiro suporta HTTP localhost nativamente (sem necessidade de HTTPS).

#### Opção `autoApprove`

| Valor | Comportamento |
|-------|---------------|
| `["*"]` | Aprova todas as ferramentas automaticamente |
| `["memory_query", "memory_status"]` | Aprova apenas as ferramentas listadas |
| `[]` | Não aprova nada — pede confirmação para cada chamada |
| *(omitido)* | Mesmo que `[]` — nenhuma aprovação automática |

Recomendação para uso somente-leitura:

```json
"autoApprove": ["memory_query", "memory_recent", "memory_status", "memory_briefing", "memory_read_page", "memory_explore"]
```

### 3.2. Verificação no Kiro

1. Salve o `mcp.json` — o Kiro reconecta automaticamente
2. No painel lateral, seção **MCP SERVERS**, deve aparecer `ai-memory Connected (N tools) ✓`

### 3.3. Configuração para outros agentes (PowerShell)

```powershell
# Claude Code
ai-memory install-mcp --client claude-code --apply
ai-memory install-hooks --agent claude-code --apply

# Cursor
ai-memory install-mcp --client cursor --apply
ai-memory install-hooks --agent cursor --apply

# Gemini CLI
ai-memory install-mcp --client gemini-cli --apply
ai-memory install-hooks --agent gemini-cli --apply
```

---

## Parte 4: Variáveis de Ambiente (opcionais)

Configurar permanentemente (PowerShell como Admin):

```powershell
# Apontar para servidor (default já é localhost:49374)
[Environment]::SetEnvironmentVariable("AI_MEMORY_SERVER_URL", "http://127.0.0.1:49374", "User")

# Token de autenticação (só se o servidor exigir)
[Environment]::SetEnvironmentVariable("AI_MEMORY_AUTH_TOKEN", "seu-token-aqui", "User")
```

Para gerar um token:
```powershell
ai-memory generate-auth-token
```

---

## Parte 5: Uso Cotidiano

### Comandos essenciais (PowerShell)

```powershell
# Status do servidor
ai-memory status

# Buscar na memória
ai-memory search "quotation approval flow"

# Ler uma página completa
ai-memory read-page --path "decisions/001-usar-postgres.md"

# Escrever uma página
ai-memory write-page --path "notes/minha-nota.md" --body "# Titulo`n`nConteudo aqui..."

# Bootstrap de projeto existente (requer LLM provider)
cd C:\Users\seu-usuario\projetos\meu-app
ai-memory bootstrap

# Audit de qualidade da wiki
ai-memory lint

# Sweep de páginas frias
ai-memory forget-sweep --dry-run
```

### Via Kiro (no chat)

- "O que fizemos na sessão passada?" → usa `memory_handoff_accept`
- "Busque nas decisões anteriores sobre autenticação" → usa `memory_query`
- "Salve essa decisão para futuras referências" → usa `memory_write_page`
- "Como está a memória do projeto?" → usa `memory_status`

---

## Parte 6: Configuração do Servidor (`config.toml`)

### Criar o `config.toml` diretamente no container

```powershell
docker exec ai-memory sh -c 'cat > /data/config.toml << EOF
# Exigir aprovação manual antes de aplicar propostas de auto-improvement
[auto_improve]
require_approval = true

# Desabilitar o scheduler automático de auto-improvement
[auto_improve.scheduler]
enabled = true
EOF'

# Reiniciar para carregar
docker restart ai-memory
```

### Verificar se o arquivo existe

```powershell
docker exec ai-memory cat /data/config.toml
```

### Combinações de `require_approval` e `scheduler.enabled`

| `require_approval` | `scheduler.enabled` | Comportamento |
|---|---|---|
| `true` | `true` | Scheduler roda e gera propostas, mas ficam **pendentes** para revisão manual |
| `true` | `false` | Nada automático — só gera quando pedir (`memory_auto_improve`) e fica pendente |
| `false` (padrão) | `true` (padrão) | Scheduler roda e aplica propostas **automaticamente** na wiki |
| `false` | `false` | Nada automático, e quando pedir manualmente aplica direto sem revisão |

> **Recomendação:** `require_approval = true` com scheduler habilitado — o sistema aprende sozinho, mas nada entra na wiki sem aprovação.

---

## Parte 7: Recriar o Container

### 7.1. Trocar configuração (preservando dados)

```powershell
# Remover container (dados ficam no volume)
docker rm -f ai-memory

# Recriar com OpenAI
docker run -d --name ai-memory `
    --restart unless-stopped `
    -p 127.0.0.1:49374:49374 `
    -v ai-memory-data:/data `
    -e AI_MEMORY_LLM_PROVIDER=openai `
    -e OPENAI_API_KEY=sk-sua-chave-aqui `
    -e AI_MEMORY_EMBEDDING_PROVIDER=openai `
    akitaonrails/ai-memory:latest
```

### 7.2. Resetar a base (apagar tudo)

```powershell
# Remover container
docker rm -f ai-memory

# Remover volume (APAGA TODOS OS DADOS)
docker volume rm ai-memory-data

# Recriar do zero
docker run -d --name ai-memory `
    --restart unless-stopped `
    -p 127.0.0.1:49374:49374 `
    -v ai-memory-data:/data `
    -e AI_MEMORY_LLM_PROVIDER=openai `
    -e OPENAI_API_KEY=sk-sua-chave-aqui `
    -e AI_MEMORY_EMBEDDING_PROVIDER=openai `
    akitaonrails/ai-memory:latest
```

### 7.3. Atualizar imagem

```powershell
docker pull akitaonrails/ai-memory:latest
docker rm -f ai-memory
# Re-executar docker run com as mesmas opções
```

### 7.4. Combinações de providers LLM

| Provider | Variáveis necessárias |
|----------|----------------------|
| OpenAI (LLM + embeddings) | `AI_MEMORY_LLM_PROVIDER=openai`, `OPENAI_API_KEY`, `AI_MEMORY_EMBEDDING_PROVIDER=openai` |
| Anthropic (LLM) + OpenAI (embeddings) | `AI_MEMORY_LLM_PROVIDER=anthropic`, `ANTHROPIC_API_KEY`, `AI_MEMORY_EMBEDDING_PROVIDER=openai`, `OPENAI_API_KEY` |
| Zero-LLM (sem chaves) | Nenhuma variável — funciona apenas busca FTS5 e handoffs |

---

## Parte 8: Troubleshooting

| Problema | Causa | Solução |
|----------|-------|---------|
| `Connection refused` | Container não está rodando | `docker start ai-memory` |
| Docker Desktop não inicia | Hyper-V desabilitado | Habilite Hyper-V nas Features do Windows |
| Kiro não mostra `ai-memory` no painel | Config JSON inválido ou servidor offline | Verifique JSON e `docker ps` |
| `'ai-memory' is not recognized` | Binary não está no PATH | Adicione `%LOCALAPPDATA%\ai-memory` ao PATH |
| Container aparece mas porta não responde | Docker Desktop parado em background | Abra Docker Desktop e aguarde inicializar |
| `exec format error` | Imagem incompatível com arquitetura | Use Docker Desktop com emulação (WSL2 backend) |
| Lentidão no startup | Docker Desktop inicializando WSL2 backend | Aguarde ~30s após boot do Windows |

---

## Diferenças entre Windows e Linux/WSL

| Aspecto | Windows Nativo | Linux/WSL |
|---------|----------------|-----------|
| Docker | Docker Desktop (GUI) | Docker Engine (daemon) |
| Binário client | `.exe` em `%LOCALAPPDATA%\ai-memory\` | Binary em `~/.local/bin/` |
| PATH | Variável de ambiente do Windows | `~/.bashrc` export |
| Hooks (agentes) | PowerShell `.ps1` | Bash `.sh` |
| Exceção | Claude Code usa Git Bash `.sh` mesmo no Windows | — |
| Performance I/O | Mais lento (volume via Hyper-V) | Nativo |

---

## Referências

- **Repositório:** https://github.com/akitaonrails/ai-memory
- **Artigo original:** https://akitaonrails.com/2026/06/16/ai-memory-memoria-longo-prazo-karpathy-wiki-auto-aprendizado-hermes-projetos/
- **Documentação Kiro MCP:** https://kiro.dev/docs/mcp/configuration/
- **Docker Desktop:** https://www.docker.com/products/docker-desktop/
