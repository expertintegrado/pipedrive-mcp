# Guia de instalação

O Pipedrive MCP é distribuído como pacote npm público: [`@expertintegrado/pipedrive-mcp`](https://www.npmjs.com/package/@expertintegrado/pipedrive-mcp). O método recomendado é o **Modo 1 (npx)** — sem download, sem clone, sem caminho absoluto. Os modos alternativos são para quem está offline, quer dev/fork, ou precisa de setup sem internet para instalar do registry.

- [Pré-requisitos](#pré-requisitos)
- [Modo 1 — npx (recomendado)](#modo-1--npx-recomendado)
- [Modo 2 — ZIP (sem git, instalação local)](#modo-2--zip-sem-git-instalação-local)
- [Modo 3 — git clone (dev / atualização via `git pull`)](#modo-3--git-clone-dev--atualização-via-git-pull)
- [Obter o token do Pipedrive](#obter-o-token-do-pipedrive)
- [Primeira sincronização](#primeira-sincronização)
- [Atualizando o MCP](#atualizando-o-mcp)

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
   - Qualquer outro cliente compatível com o protocolo MCP (Cursor, Continue, Cline, etc.)

3. **Conexão com internet** na primeira execução (para o `npx` baixar o pacote do registry). Nos modos 2 e 3, a conexão só é necessária no download inicial.

---

## Modo 1 — npx (recomendado)

Sem download manual. O `npx` baixa e executa o pacote direto do registry do npm na primeira vez que o cliente MCP inicia.

### Claude Desktop

Edite o arquivo:

- **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`
- **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Linux:** `~/.config/Claude/claude_desktop_config.json`

Ou abra via **Settings → Developer → Edit Config** no próprio Claude Desktop.

Adicione o bloco `mcpServers` (se o arquivo não existir ou estiver vazio, cole o JSON inteiro):

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

Feche e abra o Claude Desktop.

### Claude Code — por projeto

Crie o arquivo `.mcp.json` **na raiz do projeto** (não dentro de `.claude/`, não em `settings.json`):

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

Reinicie o Claude Code (encerre a sessão e abra de novo).

> ⚠️ **Não coloque `mcpServers` dentro de `settings.json`.** O Claude Code só lê MCPs do `.mcp.json` (projeto) ou do `~/.claude.json` (usuário). Qualquer `mcpServers` dentro de `settings.json` é silenciosamente ignorado.

### Claude Code — global (todos os projetos)

Use o comando oficial no terminal:

```bash
claude mcp add pipedrive -s user \
  -e PIPEDRIVE_API_KEY=seu_token_aqui \
  -e PIPEDRIVE_TIMEZONE=America/Sao_Paulo \
  -- npx -y @expertintegrado/pipedrive-mcp
```

Isso escreve no `~/.claude.json` automaticamente. Reinicie o Claude Code.

### Outros clientes MCP (Cursor, Continue, Cline etc.)

A mesma estrutura (`command: "npx"`, `args: ["-y", "@expertintegrado/pipedrive-mcp"]`, `env`) funciona. Consulte a documentação do seu cliente para saber onde fica o arquivo de configuração.

---

## Modo 2 — ZIP (sem git, instalação local)

Use quando:

- Você está em ambiente **sem acesso ao npm registry** (rede corporativa restrita, air-gapped)
- Quer **inspecionar/editar** o código antes de usar
- Quer evitar que o `npx` baixe na inicialização

### Passos

1. Baixe o ZIP: [pipedrive-mcp-main.zip](https://github.com/expertintegrado/pipedrive-mcp/archive/refs/heads/main.zip)
2. Descompacte numa pasta estável (que não será apagada). Vai gerar `pipedrive-mcp-main/` — pode renomear para `pipedrive-mcp/`.
3. Abra um terminal **dentro dessa pasta** e rode:

   ```bash
   npm install
   ```

4. Anote o **caminho absoluto** da pasta:

   ```bash
   # macOS/Linux
   pwd

   # Windows (PowerShell ou CMD)
   cd
   ```

5. Configure o cliente MCP apontando para `index.js`:

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

   > **Windows:** dentro do JSON, use barras normais `/` **ou** barras duplas `\\`. Nunca barra invertida única — quebra o parser.

6. Reinicie o cliente MCP.

Veja `examples/claude_desktop_config.local.json` e `examples/mcp.local.json` no repositório para blocos prontos.

---

## Modo 3 — git clone (dev / atualização via `git pull`)

Mesma ideia do Modo 2, mas usando git — útil se você quer rastrear mudanças no upstream com `git pull`.

```bash
git clone https://github.com/expertintegrado/pipedrive-mcp.git
cd pipedrive-mcp
npm install
```

Configure o cliente exatamente como no [Modo 2, passo 5](#modo-2--zip-sem-git-instalação-local).

---

## Obter o token do Pipedrive

1. Acesse `https://<seu-dominio>.pipedrive.com`
2. Clique na foto do seu perfil (canto superior direito)
3. **Configurações** > **Preferências pessoais** > **API**
4. Copie o **token da API pessoal**

> O token dá para quem o tiver o mesmo acesso que você tem no Pipedrive. Não compartilhe. Não commite.

---

## Primeira sincronização

Após configurar e reiniciar o cliente, peça ao Claude:

> Execute `sync_all` do Pipedrive.

O MCP vai gerar `config.js` com:

- Campos customizados de deals e contatos
- Tipos de atividade (com aliases e durações padrão)
- Pipelines e etapas
- Usuários ativos
- Domínio da empresa

**Localização do `config.js`:**

- **Modo 1 (npx):** o arquivo é criado na pasta de trabalho atual do cliente MCP (normalmente `%USERPROFILE%` no Windows, `~` no macOS/Linux). Pergunte ao Claude: *"onde o `config.js` foi salvo?"*
- **Modos 2 e 3 (local):** o arquivo vai para dentro da pasta do repositório.

Mais detalhes: [docs/SYNC.md](docs/SYNC.md).

---

## Atualizando o MCP

### Modo 1 (npx)

O `npx` verifica a versão mais recente automaticamente. Para forçar atualização imediata, limpe o cache:

```bash
npx clear-npx-cache
# ou
npm cache clean --force
```

Depois reinicie o cliente MCP. Se a atualização mudar campos/pipelines esperados, rode `sync_all` de novo.

### Modo 2 (ZIP)

Baixe o ZIP novo, substitua o conteúdo da pasta antiga pelo do ZIP novo (mantenha o **mesmo caminho absoluto** — senão precisa reconfigurar o cliente), abra o terminal na pasta e rode:

```bash
npm install
```

### Modo 3 (git)

```bash
cd CAMINHO_PARA/pipedrive-mcp
git pull
npm install
```

Em todos os casos, reinicie o cliente MCP após atualizar.

---

## Próximos passos

- Personalize regras de negócio no [CLAUDE.md.example](CLAUDE.md.example)
- Veja a lista completa de ferramentas em [docs/TOOLS.md](docs/TOOLS.md)
- Problemas? [docs/TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md)
