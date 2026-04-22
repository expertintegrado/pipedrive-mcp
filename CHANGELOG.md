# Changelog

Todas as mudanças relevantes deste projeto são documentadas aqui. O formato segue [Keep a Changelog](https://keepachangelog.com/pt-BR/1.1.0/) e o versionamento segue [SemVer](https://semver.org/lang/pt-BR/).

## [6.0.0] — 2026-04-22

### Adicionado
- Primeiro release como repositório oficial independente: `expertintegrado/pipedrive-mcp`.
- Empacotamento como pacote npm `@expertintegrado/pipedrive-mcp` com suporte a `npx`.
- Documentação separada em `docs/`: TOOLS, SYNC, TROUBLESHOOTING.
- Exemplo de configuração em `examples/claude_desktop_config.json`.

### Mudou
- Migrado do monorepo `expertintegrado/skills` (pasta `mcps/pipedrive/`) para repositório próprio.
- README reescrito com foco em instalação via `npx` (antes exigia `git clone`).
- URLs e referências internas atualizadas para o novo repositório.

## Histórico anterior

Para bugs corrigidos nas versões 5.x (quando o MCP ainda vivia no monorepo), veja o arquivo `BUG_LOG.md` no [repositório original](https://github.com/expertintegrado/skills/blob/main/mcps/pipedrive/BUG_LOG.md).

Principais marcos:

- **v5.7.x** — Guardrails anti-duplicata em `create_person`, `create_deal`, `create_organization`, `create_activity`.
- **v5.5.x** — Remoção de hardcodes de empresa (domínio dinâmico, tipos de atividade configuráveis).
- **v5.4.x** — Tradução de IDs para nomes em todas as respostas (pipelines, etapas, tipos de atividade).
- **v5.0.x** — Suporte a `config.js` unificado via `sync_all`.
