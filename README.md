# Pipedrive MCP

[![npm version](https://img.shields.io/npm/v/@expertintegrado/pipedrive-mcp.svg)](https://www.npmjs.com/package/@expertintegrado/pipedrive-mcp)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Servidor **MCP (Model Context Protocol)** para o CRM **Pipedrive**, mantido pela [Expert Integrado](https://expertintegrado.com.br). Permite que assistentes de IA — Claude Code, Claude Desktop, Cursor, etc. — interajam diretamente com o seu Pipedrive: negocios, contatos, organizacoes, atividades, notas e produtos.

Funciona com **qualquer conta** do Pipedrive. Cada pessoa configura seu proprio token; os dados de referencia (campos customizados, pipelines, etapas, tipos de atividade, usuarios) sao sincronizados automaticamente.

> Este MCP roda **localmente** na maquina de quem instalar. Nao ha servidor remoto. As credenciais ficam apenas na variavel de ambiente configurada no cliente MCP.

---

## Requisitos

- **Node.js 18+** — [instalar](https://nodejs.org/)
- Cliente MCP (ex: [Claude Code](https://claude.ai/download) ou Claude Desktop)
- Token pessoal de API do Pipedrive

## Instalacao rapida (recomendado)

Adicione ao seu arquivo de configuracao do cliente MCP:

**Claude Code** (`~/.claude/settings.json` ou `.claude/settings.json` do projeto)
**Claude Desktop** (`%APPDATA%\Claude\claude_desktop_config.json` no Windows, `~/Library/Application Support/Claude/claude_desktop_config.json` no macOS)

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

Reinicie o cliente MCP. Pronto.

> Nao quer usar `npx`? Veja [INSTALL.md](INSTALL.md) para instalacao via `git clone` ou `npm install -g`.

## Primeira sincronizacao

Na primeira vez, peca ao Claude:

> "Execute `sync_all` do Pipedrive."

Isso gera o arquivo `config.js` na pasta do MCP com os dados de referencia da sua conta. Detalhes em [docs/SYNC.md](docs/SYNC.md).

---

## Variaveis de ambiente

| Variavel | Obrigatoria | Descricao |
|----------|:-:|---|
| `PIPEDRIVE_API_KEY` | Sim | Token pessoal da API. **Pipedrive > Configuracoes > Dados pessoais > API** |
| `PIPEDRIVE_TIMEZONE` | Nao | Fuso horario (padrao: `America/Sao_Paulo`) |

## O que o MCP faz

24 ferramentas cobrindo negocios, contatos, organizacoes, atividades, notas e produtos. Inclui guardrails anti-duplicata, protecao contra sobrescrita de campos, resolucao por nome de pipelines/etapas/usuarios, dominio dinamico, conversao de fuso horario e paginacao.

Lista completa: [docs/TOOLS.md](docs/TOOLS.md).

## Seguranca

- Token **nunca** commitado no repositorio
- `config.js` (dados da conta) no `.gitignore`
- Operacoes `DELETE` bloqueadas por padrao
- Campos com valor existente protegidos contra sobrescrita
- Dominio da empresa detectado automaticamente via `/users/me`
- Deals, contatos e organizacoes criados com `visible_to: 3` (empresa inteira)

## Problemas?

Consulte [docs/TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md) ou abra uma [issue](https://github.com/expertintegrado/pipedrive-mcp/issues).

## Contribuindo

PRs sao bem-vindos. Para bugs e ideias, abra uma issue primeiro.

## Licenca

[MIT](LICENSE) © Expert Integrado
