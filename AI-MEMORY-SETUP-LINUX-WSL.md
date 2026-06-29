# ai-memory: Instalação Local Completa (Servidor + Cliente)

> Guia validado para WSL2/Linux x86_64 com Docker.  
> Última atualização: 2026-06-28

---

## Visão Geral

O ai-memory fornece memória de longo prazo para agentes de IA (Kiro, Claude Code, Cursor, etc.).  
A arquitetura é simples:

```
┌─────────────────────────────────────────────┐
│  Agente (Kiro, Claude Code, etc.)           │
│  ↕ MCP (JSON-RPC via HTTP)                  │
├─────────────────────────────────────────────┤
│  Servidor ai-memory (Docker container)      │
│  - SQLite + FTS5 (wiki/pages)               │
│  - Bind: 127.0.0.1:49374                    │
│  - Volume: ai-memory-data:/data             │
├─────────────────────────────────────────────┤
│  Cliente ai-memory (binary ~/.local/bin/)   │
│  - CLI para search, status, bootstrap, etc. │
│  - Hooks para agentes                       │
└─────────────────────────────────────────────┘
```

---

## Pré-requisitos

| Requisito | Verificação |
|-----------|-------------|
| Docker Engine | `docker --version` |
| curl | `which curl` |
| WSL2 ou Linux x86_64 | `uname -s && uname -m` |
| `~/.local/bin` no PATH | `echo $PATH \| grep local/bin` |

---

## Parte 1: Servidor (Docker)

### 1.1. Subir o container (modo zero-LLM)

```bash
docker run -d --name ai-memory \
    --restart unless-stopped \
    -p 127.0.0.1:49374:49374 \
    -v ai-memory-data:/data \
    akitaonrails/ai-memory:latest
```

> Modo zero-LLM: busca, handoffs e wiki funcionam sem chave de API.  
> Consolidação automática e embeddings ficam desabilitados.

### 1.2. (Opcional) Com LLM para consolidação

```bash
docker run -d --name ai-memory \
    --restart unless-stopped \
    -p 127.0.0.1:49374:49374 \
    -v ai-memory-data:/data \
    -e AI_MEMORY_LLM_PROVIDER=anthropic \
    -e ANTHROPIC_API_KEY=sk-ant-sua-chave-aqui \
    -e AI_MEMORY_EMBEDDING_PROVIDER=openai \
    -e OPENAI_API_KEY=sk-sua-chave-openai-aqui \
    akitaonrails/ai-memory:latest
```

### 1.3. Verificar que o servidor subiu

```bash
# Container rodando e healthy
docker ps | grep ai-memory

# Endpoint MCP respondendo (espera-se HTTP 200 ou 406 sem Accept header)
curl -sS -w "\nHTTP: %{http_code}\n" http://127.0.0.1:49374/mcp \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json, text/event-stream" \
    -d '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2024-11-05","capabilities":{},"clientInfo":{"name":"test","version":"1.0"}}}'
```

Resposta esperada: JSON com `"serverInfo":{"name":"ai-memory","version":"1.4.x"}`.

### 1.4. Gerenciamento do container

```bash
# Parar
docker stop ai-memory

# Iniciar
docker start ai-memory

# Logs
docker logs ai-memory --tail 50

# Remover (dados preservados no volume)
docker rm -f ai-memory

# Atualizar imagem
docker pull akitaonrails/ai-memory:latest
docker rm -f ai-memory
# Re-executar o docker run do passo 1.1 ou 1.2
```

### 1.5. Backup dos dados

```bash
# Backup do volume Docker
docker run --rm -v ai-memory-data:/data -v $(pwd):/backup \
    alpine tar czf /backup/ai-memory-backup-$(date +%Y%m%d).tar.gz /data
```

---

## Parte 2: Cliente (Binary Nativo)

### 2.1. Instalar o binário

```bash
mkdir -p ~/.local/bin

# Linux x86_64
curl -fsSL -o /tmp/ai-memory.tar.gz \
    "https://github.com/akitaonrails/ai-memory/releases/latest/download/ai-memory-linux-x86_64.tar.gz"
tar -xzf /tmp/ai-memory.tar.gz -C ~/.local/bin/ ai-memory
chmod +x ~/.local/bin/ai-memory
rm /tmp/ai-memory.tar.gz
```

Outros sistemas:

| Sistema | URL do release |
|---------|----------------|
| Linux x86_64 | `ai-memory-linux-x86_64.tar.gz` |
| macOS Apple Silicon | `ai-memory-macos-aarch64.tar.gz` |
| macOS Intel | `ai-memory-macos-x86_64.tar.gz` |
| Windows x86_64 | `ai-memory-windows-x86_64.zip` |

### 2.2. Garantir que está no PATH

```bash
# Se ~/.local/bin não está no PATH:
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

# Verificar
which ai-memory
ai-memory --version
```

### 2.3. Testar conexão com o servidor local

```bash
ai-memory status
```

Saída esperada:
```
ai-memory 1.4.1 (server)
  server:       http://127.0.0.1:49374
  pages:        0 (all versions: 0)
  sessions:     0
  observations: 0
```

> O cliente detecta automaticamente o servidor em `127.0.0.1:49374` (padrão).  
> Se apontar para outro host, configure via variável de ambiente (ver seção 3).

### 2.4. Configurar o projeto/workspace (`.ai-memory.toml`)

Cada repositório precisa de um arquivo `.ai-memory.toml` na raiz para identificar o projeto dentro do ai-memory. Sem ele, as observações podem cair no projeto genérico `scratch`.

```bash
# Na raiz do repositório
cat > .ai-memory.toml << 'EOF'
workspace = "default"
project = "afiliados"
EOF
```

| Campo | Descrição |
|-------|-----------|
| `workspace` | Agrupamento lógico de projetos. Use `"default"` se tem apenas um workspace |
| `project` | Nome único do projeto. Deve ser descritivo e sem espaços (use hífen ou underscore) |

> **Importante:** adicione `.ai-memory.toml` ao controle de versão (git) para que todos os devs da equipe compartilhem o mesmo identificador de projeto.

Exemplo para múltiplos projetos na mesma máquina:

```
~/repos/boaventuraseguros/Afiliados/.ai-memory.toml   → project = "afiliados"
~/repos/boaventuraseguros/Landing/.ai-memory.toml     → project = "landing"
~/repos/outro-cliente/App/.ai-memory.toml             → workspace = "outro-cliente", project = "app"
```

---

## Parte 3: Configuração do Agente (Kiro)

### 3.1. Configuração MCP

Arquivo: `.kiro/settings/mcp.json` (workspace) ou `~/.kiro/settings/mcp.json` (global)

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

Controla quais ferramentas do MCP são executadas sem pedir confirmação ao usuário:

| Valor | Comportamento |
|-------|---------------|
| `["*"]` | Aprova todas as ferramentas automaticamente (equivale ao `true` do Claude Code) |
| `["memory_query", "memory_status"]` | Aprova apenas as ferramentas listadas pelo nome |
| `[]` | Não aprova nada — pede confirmação para cada chamada |
| *(omitido)* | Mesmo que `[]` — nenhuma aprovação automática |

Recomendação: use `["*"]` para ai-memory, pois todas as operações são seguras (leitura de wiki, busca, handoffs). Se preferir controle granular, aprove apenas as de leitura:

```json
"autoApprove": ["memory_query", "memory_recent", "memory_status", "memory_briefing", "memory_read_page", "memory_explore"]
```

### 3.2. Verificação no Kiro

1. Salve o `mcp.json` — o Kiro reconecta automaticamente
2. No painel lateral, seção **MCP SERVERS**, deve aparecer `ai-memory Connected (N tools) ✓`
3. As ferramentas disponíveis incluem: `memory_query`, `memory_write_page`, `memory_status`, `memory_handoff_begin`, etc.

### 3.3. Configuração para outros agentes

```bash
# Claude Code
ai-memory install-mcp --client claude-code --apply
ai-memory install-hooks --agent claude-code --apply

# Cursor
ai-memory install-mcp --client cursor --apply
ai-memory install-hooks --agent cursor --apply

# OpenCode
ai-memory install-mcp --client opencode --apply
ai-memory install-hooks --agent opencode --apply

# Gemini CLI
ai-memory install-mcp --client gemini-cli --apply
ai-memory install-hooks --agent gemini-cli --apply
```

---

## Parte 4: Variáveis de Ambiente (opcionais)

Adicione ao `~/.bashrc` se precisar customizar:

```bash
# Apontar para servidor remoto (em vez de localhost)
export AI_MEMORY_SERVER_URL="http://127.0.0.1:49374"

# Token de autenticação (só necessário se o servidor exigir)
export AI_MEMORY_AUTH_TOKEN="seu-token-aqui"

# Diretório de dados customizado
export AI_MEMORY_DATA_DIR="$HOME/.local/share/ai-memory"
```

Para gerar um token (se quiser proteger o servidor):
```bash
ai-memory generate-auth-token
```

---

## Parte 5: Uso Cotidiano

### Comandos essenciais

```bash
# Status do servidor
ai-memory status

# Buscar na memória
ai-memory search "quotation approval flow"

# Ler uma página completa
ai-memory read-page --path "decisions/001-usar-postgres.md"

# Escrever uma página
ai-memory write-page --path "notes/minha-nota.md" \
    --body "# Título\n\nConteúdo aqui..."

# Bootstrap de projeto existente (requer LLM provider)
cd ~/projetos/meu-app
ai-memory bootstrap

# Audit de qualidade da wiki
ai-memory lint

# Sweep de páginas frias
ai-memory forget-sweep --dry-run
```

### Via Kiro (no chat)

O ai-memory é usado automaticamente pelo Kiro quando configurado. Exemplos de uso no chat:

- "O que fizemos na sessão passada?" → usa `memory_handoff_accept`
- "Busque nas decisões anteriores sobre autenticação" → usa `memory_query`
- "Salve essa decisão para futuras referências" → usa `memory_write_page`
- "Como está a memória do projeto?" → usa `memory_status`

---

## Parte 6: Troubleshooting

| Problema | Causa | Solução |
|----------|-------|---------|
| `Connection refused` no curl | Container não está rodando | `docker start ai-memory` |
| Kiro não mostra `ai-memory` no painel MCP | Config JSON inválido ou servidor offline | Verifique JSON e `docker ps` |
| `MCP error -32000: Connection closed` | Servidor não fala stdio (usa HTTP) | Use `url` no config, não `command` |
| `command not found: ai-memory` | Binary não está no PATH | `export PATH="$HOME/.local/bin:$PATH"` |
| `exec format error` | Binary errado para arquitetura | Baixe o release correto (x86_64 vs aarch64) |
| HTTP 406 ao testar endpoint | Faltou header `Accept` no curl | Adicione `-H "Accept: application/json, text/event-stream"` |
| HTTPS não funciona | Servidor não tem TLS | Use `http://` para localhost, ou configure reverse proxy para remoto |
| Container unhealthy | Porta 49374 já em uso | `lsof -i :49374` e pare o processo conflitante |

---

## Parte 7: Recriar o Container (trocar config, adicionar LLM, resetar base)

### 7.1. Parar e recriar com nova configuração (preservando dados)

```bash
# Parar e remover o container (dados ficam no volume)
docker rm -f ai-memory

# Recriar com LLM (OpenAI para embeddings)
docker run -d --name ai-memory \
    --restart unless-stopped \
    -p 127.0.0.1:49374:49374 \
    -v ai-memory-data:/data \
    -e AI_MEMORY_LLM_PROVIDER=openai \
    -e OPENAI_API_KEY=sk-sua-chave-openai-aqui \
    -e AI_MEMORY_EMBEDDING_PROVIDER=openai \
    akitaonrails/ai-memory:latest
```

> Os dados são preservados porque o volume `ai-memory-data` não é removido.

### 7.2. Resetar a base completamente (apagar tudo e começar do zero)

```bash
# 1. Remover o container
docker rm -f ai-memory

# 2. Remover o volume (APAGA TODOS OS DADOS — wiki, sessões, observações)
docker volume rm ai-memory-data

# 3. Recriar do zero
docker run -d --name ai-memory \
    --restart unless-stopped \
    -p 127.0.0.1:49374:49374 \
    -v ai-memory-data:/data \
    -e AI_MEMORY_LLM_PROVIDER=openai \
    -e OPENAI_API_KEY=sk-sua-chave-openai-aqui \
    -e AI_MEMORY_EMBEDDING_PROVIDER=openai \
    akitaonrails/ai-memory:latest
```

### 7.3. Atualizar a imagem Docker para versão mais recente

```bash
docker pull akitaonrails/ai-memory:latest
docker rm -f ai-memory

# Recriar com a mesma config (ajuste as variáveis conforme necessário)
docker run -d --name ai-memory \
    --restart unless-stopped \
    -p 127.0.0.1:49374:49374 \
    -v ai-memory-data:/data \
    akitaonrails/ai-memory:latest
```

### 7.4. Verificar volume e espaço utilizado

```bash
# Listar volumes
docker volume ls | grep ai-memory

# Ver tamanho do volume
docker system df -v | grep ai-memory
```

### 7.5. Combinações de providers LLM disponíveis

| Provider | Variáveis necessárias |
|----------|----------------------|
| OpenAI (LLM + embeddings) | `AI_MEMORY_LLM_PROVIDER=openai`, `OPENAI_API_KEY`, `AI_MEMORY_EMBEDDING_PROVIDER=openai` |
| Anthropic (LLM) + OpenAI (embeddings) | `AI_MEMORY_LLM_PROVIDER=anthropic`, `ANTHROPIC_API_KEY`, `AI_MEMORY_EMBEDDING_PROVIDER=openai`, `OPENAI_API_KEY` |
| Zero-LLM (sem chaves) | Nenhuma variável — funciona apenas busca FTS5 e handoffs |

### 7.6. Configuração do servidor via `config.toml`

O servidor ai-memory aceita um arquivo de configuração opcional. Dentro do container Docker, ele fica em `/data/config.toml`. Para editar:

```bash
# Acessar o container
docker exec -it ai-memory sh

# Editar (ou criar) o config
vi /data/config.toml
```

Ou montar um arquivo local:

```bash
docker rm -f ai-memory

docker run -d --name ai-memory \
    --restart unless-stopped \
    -p 127.0.0.1:49374:49374 \
    -v ai-memory-data:/data \
    -v ~/.config/ai-memory/config.toml:/data/config.toml:ro \
    akitaonrails/ai-memory:latest
```

#### Opções úteis do `config.toml`

```toml
# Exigir aprovação manual antes de aplicar propostas de auto-improvement
[auto_improve]
require_approval = true

# Desabilitar o scheduler automático de auto-improvement
[auto_improve.scheduler]
enabled = false
```

#### Criar o `config.toml` diretamente no container (sem recriar)

```bash
docker exec ai-memory sh -c 'cat > /data/config.toml << EOF
# Exigir aprovação manual antes de aplicar propostas de auto-improvement
[auto_improve]
require_approval = true

# Desabilitar o scheduler automático de auto-improvement
[auto_improve.scheduler]
enabled = true
EOF'

# Reiniciar para carregar a nova config
docker restart ai-memory
```

#### Verificar se o arquivo existe

```bash
docker exec ai-memory cat /data/config.toml
```

#### Combinações de `require_approval` e `scheduler.enabled`

| `require_approval` | `scheduler.enabled` | Comportamento |
|---|---|---|
| `true` | `true` | Scheduler roda e gera propostas, mas ficam **pendentes** para revisão manual |
| `true` | `false` | Nada automático — só gera quando pedir (`memory_auto_improve`) e fica pendente |
| `false` (padrão) | `true` (padrão) | Scheduler roda e aplica propostas **automaticamente** na wiki |
| `false` | `false` | Nada automático, e quando pedir manualmente aplica direto sem revisão |

> **Recomendação:** `require_approval = true` com scheduler habilitado é a combinação mais segura — o sistema continua aprendendo sozinho, mas nada entra na wiki sem você aprovar.

---

## Referências

- **Repositório:** https://github.com/akitaonrails/ai-memory
- **Artigo original:** https://akitaonrails.com/2026/06/16/ai-memory-memoria-longo-prazo-karpathy-wiki-auto-aprendizado-hermes-projetos/
- **Documentação Kiro MCP:** https://kiro.dev/docs/mcp/configuration/
