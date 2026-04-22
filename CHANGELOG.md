# Changelog

Todas as mudancas relevantes deste projeto sao documentadas aqui. O formato segue [Keep a Changelog](https://keepachangelog.com/pt-BR/1.1.0/) e o versionamento segue [SemVer](https://semver.org/lang/pt-BR/).

## [6.0.0] — 2026-04-22

### Adicionado
- Primeiro release como repositorio oficial independente: `expertintegrado/pipedrive-mcp`.
- Empacotamento como pacote npm `@expertintegrado/pipedrive-mcp` com suporte a `npx`.
- Documentacao separada em `docs/`: TOOLS, SYNC, TROUBLESHOOTING.
- Exemplo de configuracao em `examples/claude_desktop_config.json`.

### Mudou
- Migrado do monorepo `expertintegrado/skills` (pasta `mcps/pipedrive/`) para repositorio proprio.
- README reescrito com foco em instalacao via `npx` (antes exigia `git clone`).
- URLs e referencias internas atualizadas para o novo repositorio.

## Historico anterior

Para bugs corrigidos nas versoes 5.x (quando o MCP ainda vivia no monorepo), veja o arquivo `BUG_LOG.md` no [repositorio original](https://github.com/expertintegrado/skills/blob/main/mcps/pipedrive/BUG_LOG.md).

Principais marcos:

- **v5.7.x** — Guardrails anti-duplicata em `create_person`, `create_deal`, `create_organization`, `create_activity`.
- **v5.5.x** — Remocao de hardcodes de empresa (dominio dinamico, tipos de atividade configuraveis).
- **v5.4.x** — Traducao de IDs para nomes em todas as respostas (pipelines, etapas, tipos de atividade).
- **v5.0.x** — Suporte a `config.js` unificado via `sync_all`.
