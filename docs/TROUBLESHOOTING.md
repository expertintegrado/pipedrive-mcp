# Troubleshooting

## O MCP não aparece no Claude

1. Confira que o JSON do arquivo de configuração está válido (sem vírgula sobrando, aspas corretas). Um JSON quebrado é silenciosamente ignorado.
2. Confira se o arquivo é o **certo pro seu cliente**:
   - **Claude Code, projeto:** `.mcp.json` na raiz do projeto (**não** `.claude/settings.json`).
   - **Claude Code, global:** `~/.claude.json` (use `claude mcp add` — veja [INSTALL.md](../INSTALL.md#claude-code--global-todos-os-projetos)).
   - **Claude Desktop:** `claude_desktop_config.json` (caminho por SO em [INSTALL.md](../INSTALL.md#claude-desktop)).
   > ⚠️ `mcpServers` dentro de `settings.json` no Claude Code **é ignorado**. Só funciona em `.mcp.json` ou `~/.claude.json`.
3. Reinicie completamente o Claude Code / Claude Desktop (encerre a sessão/processo e abra de novo).
4. No Claude Desktop, abra **Developer > Open MCP Log File** para ver erros de inicialização.
5. No Claude Code, rode `/mcp` — deve listar `pipedrive` como conectado.

## Erro: `Cannot find module` ou `MODULE_NOT_FOUND`

O `npm install` não rodou (ou rodou na pasta errada). Entre na pasta do repositório clonado e rode:

```bash
cd CAMINHO/pipedrive-mcp
npm install
```

Confira que a pasta `node_modules` existe dentro de `pipedrive-mcp/`. Reinicie o cliente depois.

## Erro: `node: command not found` (ou equivalente no Windows)

Node.js não está instalado ou não está no PATH. Rode `node --version` num terminal novo. Se der erro, [instale o Node 18+](https://nodejs.org/) e reinicie o computador.

## Erro: caminho não encontrado no `args`

O caminho absoluto para `index.js` está errado. Verifique:

- Está apontando pra **`index.js`** (arquivo), não pra pasta.
- No **Windows**, dentro do JSON, use `/` ou `\\` — **nunca** `\` sozinho (quebra o parser).
- O arquivo existe: `node CAMINHO/pipedrive-mcp/index.js` deveria iniciar o servidor (ele fica parado aguardando stdio — pode matar com Ctrl+C).

## Erro: `PIPEDRIVE_API_KEY not set`

O token não chegou como variável de ambiente. Verifique se o bloco `env` está **dentro** de `pipedrive` (não no nível raiz do JSON) e se o cliente foi reiniciado após salvar.

## Erro: `401 Unauthorized` ou `403 Forbidden`

- Token expirado ou revogado. Gere outro em **Pipedrive > Configurações > Preferências pessoais > API**.
- Token de conta sem permissão para a operação (ex: usuário não-admin tentando criar campos).

## Respostas mostram IDs em vez de nomes

O `config.js` não foi carregado. Rode `sync_all` — veja [SYNC.md](SYNC.md).

## Atividade criada em horário errado (+/- 3h)

Verifique `PIPEDRIVE_TIMEZONE`. O padrão é `America/Sao_Paulo`. Se você está em outro fuso, ajuste:

```json
"env": {
  "PIPEDRIVE_API_KEY": "...",
  "PIPEDRIVE_TIMEZONE": "America/New_York"
}
```

Use o [nome IANA](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) do fuso.

## `sync_all` falha com timeout

Contas com muitos usuários/campos podem estourar o tempo. Tente novamente. Se persistir, abra uma [issue](https://github.com/expertintegrado/pipedrive-mcp/issues) com o erro completo.

## Mudei algo no Pipedrive e o MCP não reconhece

Rode `sync_all` de novo para regenerar o `config.js`. Depois reinicie o cliente MCP (o arquivo é lido no startup).

## Vi alguém usar `npx @expertintegrado/pipedrive-mcp` — por que não funciona?

O pacote **ainda não está publicado** no npm registry. Por enquanto, instale clonando o repositório — veja [INSTALL.md](../INSTALL.md). Quando publicarmos, o `npx` passa a ser uma opção.

## Links das respostas apontam para o domínio errado

O domínio é detectado automaticamente via `/users/me` no startup. Se está errado:
1. Confirme que o token pertence à conta certa.
2. Rode `sync_all` para atualizar `config.js`.
3. Reinicie o cliente.

## Atualizei com `git pull` e deu erro

Rode `npm install` de novo — pode ter entrado dependência nova. Depois reinicie o cliente.

## Não encontrou a resposta?

[Abra uma issue](https://github.com/expertintegrado/pipedrive-mcp/issues) descrevendo:
- O erro exato (copie do log)
- Versão do Node (`node --version`)
- Qual cliente MCP você usa (Claude Desktop, Claude Code, Cursor etc.)
- Sistema operacional
