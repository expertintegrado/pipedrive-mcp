# Guia de instalacao

Este guia cobre os tres modos de instalacao do Pipedrive MCP.

- [Modo 1 — `npx` (recomendado)](#modo-1--npx-recomendado)
- [Modo 2 — `npm install -g`](#modo-2--npm-install--g)
- [Modo 3 — `git clone` (contribuidores)](#modo-3--git-clone-contribuidores)
- [Obtendo o token do Pipedrive](#obtendo-o-token-do-pipedrive)
- [Configurando o cliente MCP](#configurando-o-cliente-mcp)
- [Primeira sincronizacao](#primeira-sincronizacao)

---

## Pre-requisitos

1. **Node.js 18 ou superior**. Verifique com:
   ```bash
   node --version
   ```
   Nao tem? [Baixar aqui](https://nodejs.org/). Reinicie o computador apos instalar.

2. **Um cliente MCP** instalado, tipicamente:
   - [Claude Code](https://claude.ai/download) (CLI no terminal)
   - [Claude Desktop](https://claude.ai/download) (app desktop)

## Modo 1 — `npx` (recomendado)

Nao precisa clonar nada. O cliente MCP baixa o pacote automaticamente na primeira execucao.

Pule direto para [Configurando o cliente MCP](#configurando-o-cliente-mcp) usando este bloco:

```json
{
  "mcpServers": {
    "pipedrive": {
      "command": "npx",
      "args": ["-y", "@expertintegrado/pipedrive-mcp"],
      "env": {
        "PIPEDRIVE_API_KEY": "seu_token_aqui",
        "PIPEDRIVE_TIMEZONE": "America/Sao_Paulo"
      }
    }
  }
}
```

## Modo 2 — `npm install -g`

Se voce nao quer depender do `npx` baixar toda vez:

```bash
npm install -g @expertintegrado/pipedrive-mcp
```

E use este bloco:

```json
{
  "mcpServers": {
    "pipedrive": {
      "command": "pipedrive-mcp",
      "env": {
        "PIPEDRIVE_API_KEY": "seu_token_aqui",
        "PIPEDRIVE_TIMEZONE": "America/Sao_Paulo"
      }
    }
  }
}
```

## Modo 3 — `git clone` (contribuidores)

Para desenvolver/modificar o MCP:

```bash
git clone https://github.com/expertintegrado/pipedrive-mcp.git
cd pipedrive-mcp
npm install
```

E aponte o cliente para o caminho local:

```json
{
  "mcpServers": {
    "pipedrive": {
      "command": "node",
      "args": ["/caminho/absoluto/para/pipedrive-mcp/index.js"],
      "env": {
        "PIPEDRIVE_API_KEY": "seu_token_aqui",
        "PIPEDRIVE_TIMEZONE": "America/Sao_Paulo"
      }
    }
  }
}
```

> No Windows, use barras normais `/` ou barras duplas `\\` no caminho dentro do JSON.

---

## Obtendo o token do Pipedrive

1. Acesse `https://<seu-dominio>.pipedrive.com`
2. Clique na foto do seu perfil (canto superior direito)
3. **Configuracoes** > **Preferencias pessoais** > **API**
4. Copie o **token da API pessoal**

> O token da para quem o tiver o mesmo acesso que voce tem no Pipedrive. Nao compartilhe. Nao commite.

## Configurando o cliente MCP

### Claude Code

Edite `~/.claude/settings.json` (global) ou `.claude/settings.json` (por projeto) e adicione o bloco `mcpServers`. Depois reinicie o Claude Code.

### Claude Desktop

- **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`
- **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Linux:** `~/.config/Claude/claude_desktop_config.json`

Depois feche e abra o Claude Desktop.

### Outros clientes MCP

A mesma estrutura (`command`, `args`, `env`) funciona em Cursor, Continue, Cline e outros clientes compativeis com MCP.

Um exemplo completo de `claude_desktop_config.json` esta em [examples/claude_desktop_config.json](examples/claude_desktop_config.json).

## Primeira sincronizacao

Apos configurar, peca ao Claude:

> Execute `sync_all` do Pipedrive.

O MCP vai gerar `config.js` na pasta onde o servidor esta rodando, contendo:

- Campos customizados de deals e contatos
- Tipos de atividade (com aliases e duracoes padrao)
- Pipelines e etapas
- Usuarios ativos
- Dominio da empresa

Mais detalhes: [docs/SYNC.md](docs/SYNC.md).

## Proximos passos

- Personalize regras de negocio no [CLAUDE.md.example](CLAUDE.md.example)
- Veja a lista completa de ferramentas em [docs/TOOLS.md](docs/TOOLS.md)
- Problemas? [docs/TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md)
