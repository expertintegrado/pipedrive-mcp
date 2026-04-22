# Instruções para o Claude Code neste repositório

Este arquivo é lido automaticamente pelo Claude Code quando alguém abre este repositório. Ele direciona tarefas comuns para a documentação detalhada correspondente.

> **Não confunda com `CLAUDE.md.example`.** Aquele é um exemplo que o usuário final do MCP copia pro próprio projeto pessoal (regras de uso do Pipedrive no dia a dia). Este aqui é para quem desenvolve o MCP.

---

## Mapa de tarefas

| Quando o usuário pedir... | Leia e siga |
|---|---|
| "Implemente X", "corrija o bug Y", "adicione a feature Z" | [CONTRIBUTING.md](CONTRIBUTING.md) |
| "Faça uma release", "publica uma versão nova", "bump patch/minor/major" | [RELEASING.md](RELEASING.md) |
| "Como instalar/configurar o MCP" (usuário perguntando de fora) | [README.md](README.md) — método primário é `npx`, fallback manual em `<details>` |
| "Não está funcionando", erros em runtime | [docs/TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md) |
| "O que faz a tool X?", "quais tools existem?" | [docs/TOOLS.md](docs/TOOLS.md) |
| "Rodar sync_all", "mexer no config.js" | [docs/SYNC.md](docs/SYNC.md) |

Quando a tarefa casar com uma linha acima, leia o arquivo referenciado **antes** de começar. Os docs são escritos para execução direta — contêm pré-requisitos verificáveis, comandos copiáveis e procedimentos de recuperação.

---

## Invariantes do repositório

Violações destas regras devem ser questionadas com o usuário antes de prosseguir:

1. **Nunca push direto na `main`.** Toda mudança passa por Pull Request.
2. **Nunca `npm publish` manual.** Publicação é automática via workflow `.github/workflows/release.yml` disparado por push de tag `v*`.
3. **`package.json.version`, tag git `vX.Y.Z` e entrada em `CHANGELOG.md` precisam estar sempre sincronizados.** O workflow valida — se desincronizar, falha.
4. **Sem segredos no código, testes ou exemplos.** Tokens, credenciais, dados reais de conta — jamais comitar.
5. **Dependências mínimas.** O pacote é instalado via `npx` — cada dep adicional atrasa startup pros usuários. Adicione só se justificável.
6. **ESM only** (`"type": "module"`), **Node.js 18+**, sem build step — o `index.js` roda direto.

---

## Contexto rápido do projeto

- **O que é:** servidor MCP (Model Context Protocol) que expõe o CRM Pipedrive para clientes Claude (Claude Code, Claude Desktop, Cursor, etc.)
- **Onde está publicado:** [`@expertintegrado/pipedrive-mcp`](https://www.npmjs.com/package/@expertintegrado/pipedrive-mcp) no npm
- **Método primário de instalação:** `npx -y @expertintegrado/pipedrive-mcp` — sem clone, sem caminho absoluto
- **Arquivo principal:** [`index.js`](index.js) — servidor MCP completo
- **Geração de config dinâmico:** [`add-descriptions.js`](add-descriptions.js) (usado no build interno de descrições)
- **Runtime config da conta do usuário:** `config.js` — gerado pela tool `sync_all`, contém campos/pipelines/usuários do Pipedrive do usuário (NÃO é commitado — está no `.gitignore`)

---

## Preferências de interação

- Respostas e commits em **português brasileiro**
- Commits: imperativo, foco no **porquê** (não no "o quê")
- Prefixos de commit: `feat:`, `fix:`, `docs:`, `chore:`, `refactor:`, `test:`
- Ao terminar tarefas com múltiplas mudanças, rode `git status` e confirme antes de commitar
- Ao rodar comandos de release, sempre faça as verificações de pré-requisito do [RELEASING.md](RELEASING.md) antes — nunca pule
