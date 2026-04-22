# Contribuindo com o Pipedrive MCP

Obrigado por querer contribuir! Este guia cobre o fluxo de trabalho para mudanças neste repositório.

## Princípios

- **Nada vai direto pra `main`.** Toda mudança passa por Pull Request e revisão.
- **Publicação no npm é automática** — quem cria a tag `v*` publica. Não existe `npm publish` manual.
- **Mantenha `package.json`, `CHANGELOG.md` e a tag git sempre sincronizados.** O workflow falha se a versão do `package.json` não bater com a tag da release.

## Fluxo de trabalho

### 1. Crie uma branch a partir da `main`

Use um dos prefixos convencionais:

```bash
git checkout main
git pull
git checkout -b feat/nome-curto-da-feature
# ou fix/descricao-do-bug
# ou docs/o-que-mudou
# ou chore/tarefa-tecnica
```

### 2. Faça as mudanças e commite

Mensagens de commit em português, no imperativo, descrevendo o **porquê** mais do que o **o quê**:

```bash
git commit -m "fix: corrigir fuso horário em create_activity quando PIPEDRIVE_TIMEZONE vazio"
```

Prefixos recomendados: `feat:`, `fix:`, `docs:`, `chore:`, `refactor:`, `test:`.

### 3. Se a mudança for publicável, atualize o `CHANGELOG.md`

Toda mudança que vai sair em release precisa de uma entrada no `CHANGELOG.md`, adicionada **no topo** do arquivo (abaixo do cabeçalho), numa seção `## [Unreleased]` — ou, se já for a release, na seção da versão que vai ser publicada.

Seções padrão (Keep a Changelog):

- **Adicionado** — funcionalidades novas
- **Mudou** — alterações em funcionalidades existentes
- **Corrigido** — bugs corrigidos
- **Removido** — funcionalidades removidas
- **Segurança** — correções de segurança

### 4. Abra um Pull Request

- **Alvo:** `main`
- **Título:** curto, descritivo (o template do PR vai pedir mais detalhes no corpo)
- **Preencha o checklist** do template — o revisor vai conferir

### 5. Revisão

- Pelo menos **1 aprovação** é obrigatória antes de merge.
- Se houver comentários pedindo mudança, faça os ajustes na mesma branch e peça re-review.

### 6. Merge

Quem aprovou (ou você, após aprovação) faz o merge via **Squash and merge** (preferencial — mantém a `main` linear).

**Atenção:** merge em `main` **não publica nada**. Só prepara o código pra próxima release. Pra publicar, siga o [RELEASING.md](RELEASING.md).

## Estilo de código

- **JavaScript ESM** (`"type": "module"` no `package.json`)
- **Node.js 18+** — pode usar features modernas (top-level `await`, `nullish coalescing`, etc.)
- **Sem transpilação** — rode direto com `node index.js`
- **Mantenha dependências mínimas** — o pacote é instalado via `npx`, menos dependência = startup mais rápido

## Testando localmente

Antes de abrir PR, teste que o MCP inicia sem erro:

```bash
PIPEDRIVE_API_KEY=seu_token node index.js
```

(O servidor fica aguardando stdio — use Ctrl+C pra encerrar.)

Pra testar de ponta a ponta como usuário final:

1. Aponte o seu Claude Code local pra esta pasta em vez do pacote npm:
   ```bash
   claude mcp add pipedrive-dev -s user \
     -e PIPEDRIVE_API_KEY=seu_token \
     -- node /caminho/absoluto/pipedrive-mcp/index.js
   ```
2. Reinicie o Claude Code e teste o MCP normalmente.

## Dúvidas

- Problema com instalação: [docs/TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md)
- Procedimento de release: [RELEASING.md](RELEASING.md)
- Outras: [abra uma issue](https://github.com/expertintegrado/pipedrive-mcp/issues/new/choose)
