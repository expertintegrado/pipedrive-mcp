# Guia de instalação

O Pipedrive MCP é instalado **baixando este repositório** e apontando o cliente MCP para o `index.js` local. O pacote ainda **não está publicado no npm** — qualquer instrução que você veja recomendando `npx @expertintegrado/pipedrive-mcp` ou `npm install -g @expertintegrado/pipedrive-mcp` **não vai funcionar por enquanto**.

- [Pré-requisitos](#pré-requisitos)
- [Passo 1 — Baixar o código e instalar dependências](#passo-1--baixar-o-código-e-instalar-dependências)
- [Passo 2 — Obter o token do Pipedrive](#passo-2--obter-o-token-do-pipedrive)
- [Passo 3 — Configurar o cliente MCP](#passo-3--configurar-o-cliente-mcp)
- [Passo 4 — Primeira sincronização](#passo-4--primeira-sincronização)
- [Atualizando o MCP](#atualizando-o-mcp)
- [Modo futuro: npm](#modo-futuro-npm)

---

## Pré-requisitos

1. **Node.js 18 ou superior.** Verifique com:
   ```bash
   node --version
   ```
   Não tem? [Baixar aqui](https://nodejs.org/). Reinicie o computador após instalar.

2. **Um cliente MCP** instalado:
   - [Claude Desktop](https://claude.ai/download) (app desktop)
   - [Claude Code](https://claude.ai/download) (CLI no terminal)

3. **Git (opcional).** Só é necessário se você quiser usar a Opção B no Passo 1 (atualizações via `git pull`). Sem git, use a Opção A (ZIP).

## Passo 1 — Baixar o código e instalar dependências

Escolha uma pasta estável onde o MCP vai ficar (ele roda a partir dela para sempre — não apague depois).

### Opção A — ZIP (sem git)

1. Baixe: [pipedrive-mcp-main.zip](https://github.com/expertintegrado/pipedrive-mcp/archive/refs/heads/main.zip)
2. Descompacte na pasta escolhida. Vai gerar `pipedrive-mcp-main/` (pode renomear para `pipedrive-mcp/` se preferir).
3. Abra um terminal **dentro dessa pasta** e rode:

   ```bash
   npm install
   ```

### Opção B — git clone

```bash
git clone https://github.com/expertintegrado/pipedrive-mcp.git
cd pipedrive-mcp
npm install
```

### Em seguida (qualquer opção)

Anote o **caminho absoluto** da pasta — você vai usar no Passo 3. Para ver:

```bash
# macOS/Linux
pwd

# Windows (PowerShell ou CMD)
cd
```

Exemplo de caminhos que você pode ter:

- macOS/Linux: `/Users/seu-usuario/Documents/pipedrive-mcp`
- Windows: `C:\Users\seu-usuario\Documents\pipedrive-mcp`

## Passo 2 — Obter o token do Pipedrive

1. Acesse `https://<seu-dominio>.pipedrive.com`
2. Clique na foto do seu perfil (canto superior direito)
3. **Configurações** > **Preferências pessoais** > **API**
4. Copie o **token da API pessoal**

> O token dá para quem o tiver o mesmo acesso que você tem no Pipedrive. Não compartilhe. Não commite.

## Passo 3 — Configurar o cliente MCP

Cada cliente tem um arquivo diferente. Use o bloco da seção do seu cliente.

Em todos os casos, substitua:

- `CAMINHO_ABSOLUTO_PARA/pipedrive-mcp/index.js` pelo caminho real do Passo 1 (aponte para o `index.js` dentro da pasta clonada)
- `seu_token_aqui` pelo token do Passo 2

> **Windows:** dentro do JSON, use barras normais `/` **ou** barras duplas `\\`. Nunca barra invertida única — quebra o parser.

### Claude Desktop

Edite o arquivo:

- **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`
- **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Linux:** `~/.config/Claude/claude_desktop_config.json`

Adicione o bloco `mcpServers` (se o arquivo não existir, crie com este conteúdo):

```json
{
  "mcpServers": {
    "pipedrive": {
      "command": "node",
      "args": ["CAMINHO_ABSOLUTO_PARA/pipedrive-mcp/index.js"],
      "env": {
        "PIPEDRIVE_API_KEY": "seu_token_aqui",
        "PIPEDRIVE_TIMEZONE": "America/Sao_Paulo"
      }
    }
  }
}
```

Feche e abra o Claude Desktop.

### Claude Code — por projeto

Crie o arquivo `.mcp.json` **na raiz do projeto** (não dentro de `.claude/`, não em `settings.json`):

```json
{
  "mcpServers": {
    "pipedrive": {
      "command": "node",
      "args": ["CAMINHO_ABSOLUTO_PARA/pipedrive-mcp/index.js"],
      "env": {
        "PIPEDRIVE_API_KEY": "seu_token_aqui",
        "PIPEDRIVE_TIMEZONE": "America/Sao_Paulo"
      }
    }
  }
}
```

Reinicie o Claude Code (encerre a sessão e abra de novo).

> ⚠️ **Não coloque `mcpServers` dentro de `settings.json`.** O Claude Code só lê MCPs do `.mcp.json` (projeto) ou do `~/.claude.json` (usuário). Qualquer `mcpServers` dentro de `settings.json` é silenciosamente ignorado.

### Claude Code — global (todos os projetos)

Use o comando oficial no terminal:

```bash
claude mcp add pipedrive -s user \
  -e PIPEDRIVE_API_KEY=seu_token_aqui \
  -e PIPEDRIVE_TIMEZONE=America/Sao_Paulo \
  -- node CAMINHO_ABSOLUTO_PARA/pipedrive-mcp/index.js
```

Isso escreve no `~/.claude.json` automaticamente. Reinicie o Claude Code.

### Outros clientes MCP (Cursor, Continue, Cline etc.)

A mesma estrutura (`command: "node"`, `args: ["caminho/index.js"]`, `env`) funciona. Consulte a documentação do seu cliente para saber onde fica o arquivo de configuração.

## Passo 4 — Primeira sincronização

Após configurar e reiniciar o cliente, peça ao Claude:

> Execute `sync_all` do Pipedrive.

O MCP vai gerar `config.js` **dentro da pasta do repositório clonado**, contendo:

- Campos customizados de deals e contatos
- Tipos de atividade (com aliases e durações padrão)
- Pipelines e etapas
- Usuários ativos
- Domínio da empresa

Mais detalhes: [docs/SYNC.md](docs/SYNC.md).

## Atualizando o MCP

Quando sair versão nova:

**Se você usou a Opção A (ZIP):** baixe o ZIP novo, substitua o conteúdo da pasta antiga pelo conteúdo do ZIP novo (mantenha o **mesmo caminho absoluto** — senão precisa reconfigurar o cliente), abra o terminal na pasta e rode:

```bash
npm install
```

**Se você usou a Opção B (git):**

```bash
cd CAMINHO_PARA/pipedrive-mcp
git pull
npm install
```

Em ambos os casos, reinicie o cliente MCP. Se a atualização mudar campos/pipelines esperados, rode `sync_all` de novo.

## Modo futuro: npm

Quando o pacote for publicado em registry, vai ser possível trocar o bloco por:

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

Até lá, use o modo local descrito acima.

## Próximos passos

- Personalize regras de negócio no [CLAUDE.md.example](CLAUDE.md.example)
- Veja a lista completa de ferramentas em [docs/TOOLS.md](docs/TOOLS.md)
- Problemas? [docs/TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md)
