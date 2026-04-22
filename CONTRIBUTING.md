# Contribuindo

Este documento é executável por uma IA. Se você é humano, peça ao Claude Code: *"Implemente `<descrição da mudança>` seguindo o CONTRIBUTING.md."*

## Regras fixas

1. **Nada vai direto pra `main`.** Toda mudança passa por Pull Request.
2. **Não rode `npm publish` manualmente** — quem publica é o workflow `release.yml` via push de tag. Ver [RELEASING.md](RELEASING.md).
3. **Não inclua segredos.** Tokens, credenciais, dados de conta — nunca no código, nos testes, nos exemplos ou nos commits.
4. **Dependências mínimas.** Cada dependência nova aumenta o tempo de startup via `npx`. Adicione apenas se justificável.

## Fluxo

### 1. Criar branch a partir da `main`

Prefixos convencionais: `feat/`, `fix/`, `docs/`, `chore/`, `refactor/`, `test/`.

```bash
git checkout main
git pull
git checkout -b <prefixo>/<nome-curto>
```

### 2. Implementar e commitar

Mensagens em português, imperativo, foco no porquê:

```bash
git commit -m "fix: corrige fuso horario em create_activity quando PIPEDRIVE_TIMEZONE vazio"
```

### 3. Testar localmente

```bash
# O MCP deve iniciar sem erro (fica aguardando stdio — Ctrl+C pra encerrar)
PIPEDRIVE_API_KEY=<token_de_teste> node index.js
```

Pra testar de ponta a ponta como usuário final, registre o MCP local no Claude Code:

```bash
claude mcp add pipedrive-dev -s user \
  -e PIPEDRIVE_API_KEY=<token> \
  -- node <caminho-absoluto>/pipedrive-mcp/index.js
```

Reinicie o Claude Code e valide as ferramentas afetadas pela sua mudança.

### 4. Atualizar documentação

Se a mudança altera comportamento visível ao usuário, atualize na mesma PR:

- `README.md` — se muda instalação, uso básico, ou lista de features
- `INSTALL.md` — se muda setup, pré-requisitos, ou config
- `docs/TOOLS.md` — se adiciona, remove ou muda uma tool
- `docs/TROUBLESHOOTING.md` — se a mudança resolve ou introduz caso de erro conhecido
- `docs/SYNC.md` — se muda o comportamento de `sync_all` ou `config.js`

**Não atualize `CHANGELOG.md` aqui** — isso é feito no momento da release (ver [RELEASING.md](RELEASING.md)), consolidando todos os PRs da versão.

### 5. Abrir Pull Request

```bash
git push -u origin HEAD
gh pr create --fill   # ou use a UI do GitHub
```

O template do PR aparece automaticamente. Preencha o checklist.

### 6. Revisão + merge

- Pelo menos **1 aprovação** obrigatória.
- Merge via **Squash and merge** (mantém histórico da `main` linear).
- Após merge, a mudança está na `main` mas **não publicada**. Publicação é passo separado — ver [RELEASING.md](RELEASING.md).

## Estilo técnico

- JavaScript ESM (`"type": "module"`)
- Node.js 18+ — features modernas liberadas (top-level await, etc.)
- Sem transpilação, sem build step — roda direto com `node index.js`

## Dúvidas

- Instalação / erros de runtime: [docs/TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md)
- Publicação de versão nova: [RELEASING.md](RELEASING.md)
- Outras: [abra uma issue](https://github.com/expertintegrado/pipedrive-mcp/issues/new/choose)
