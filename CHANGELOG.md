# Changelog

Todas as mudanças relevantes deste projeto são documentadas aqui. O formato segue [Keep a Changelog](https://keepachangelog.com/pt-BR/1.1.0/) e o versionamento segue [SemVer](https://semver.org/lang/pt-BR/).

## [6.0.1] — 2026-04-22

### Mudou
- **Documentação reescrita para instalação local** — o pacote ainda não está publicado no npm, então toda a doc apontando para `npx @expertintegrado/pipedrive-mcp` ou `npm install -g` travava quem seguia o README. `INSTALL.md`, `README.md` e `docs/TROUBLESHOOTING.md` agora descrevem o fluxo local (download → `npm install` → apontar `command: "node"` + caminho absoluto).
- **Git deixou de ser obrigatório.** README e `INSTALL.md` agora oferecem duas opções: baixar o ZIP direto do GitHub (sem git) ou `git clone` (mais prático pra atualizar depois). A opção ZIP é a primária no README pra reduzir dependências em usuários não-dev.
- **Corrigida instrução errada para Claude Code:** a doc antiga mandava colocar `mcpServers` em `.claude/settings.json`, mas o Claude Code só lê MCPs de `.mcp.json` (projeto) ou `~/.claude.json` (usuário) — `settings.json` era silenciosamente ignorado. Todas as referências foram corrigidas e há um aviso explícito no `INSTALL.md` e no `TROUBLESHOOTING.md`.
- Adicionado `examples/mcp.json` como exemplo específico para Claude Code.
- `examples/claude_desktop_config.json` agora usa `command: "node"` com caminho absoluto, não `npx`.

## [6.0.0] — 2026-04-22

### Adicionado
- Primeiro release como repositório oficial independente: `expertintegrado/pipedrive-mcp`.
- Empacotamento como pacote npm `@expertintegrado/pipedrive-mcp` com suporte a `npx` (pacote **não publicado** ainda — ver 6.0.1).
- Documentação separada em `docs/`: TOOLS, SYNC, TROUBLESHOOTING.
- Exemplo de configuração em `examples/claude_desktop_config.json`.

### Mudou
- Migrado do monorepo `expertintegrado/skills` (pasta `mcps/pipedrive/`) para repositório próprio.
- README reescrito com foco em instalação via `npx` (antes exigia `git clone`) — **revertido em 6.0.1** porque o pacote não foi publicado.
- URLs e referências internas atualizadas para o novo repositório.

## Histórico anterior

Para bugs corrigidos nas versões 5.x (quando o MCP ainda vivia no monorepo), veja o arquivo `BUG_LOG.md` no [repositório original](https://github.com/expertintegrado/skills/blob/main/mcps/pipedrive/BUG_LOG.md).

Principais marcos:

- **v5.7.x** — Guardrails anti-duplicata em `create_person`, `create_deal`, `create_organization`, `create_activity`.
- **v5.5.x** — Remoção de hardcodes de empresa (domínio dinâmico, tipos de atividade configuráveis).
- **v5.4.x** — Tradução de IDs para nomes em todas as respostas (pipelines, etapas, tipos de atividade).
- **v5.0.x** — Suporte a `config.js` unificado via `sync_all`.
