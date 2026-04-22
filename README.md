# Pipedrive MCP

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Conecta o seu **Pipedrive** ao **Claude** (Claude Code ou Claude Desktop). Depois de instalar, você pode pedir ao Claude coisas como *"cria um deal pro cliente X"*, *"quais atividades eu tenho hoje?"*, *"adiciona uma nota no deal 123"* — e ele faz, direto no seu Pipedrive.

> A instalação é feita clonando este repositório (ainda não publicamos no npm). Todo o processo abaixo você faz uma vez só, em ~5 minutos.

---

## Passo 1 — Instale o que precisa

Baixe e instale (uma vez só, na sua máquina):

- [Node.js 18 ou superior](https://nodejs.org/) — baixe e clique "Avançar" até o fim. **Reinicie o computador** depois.
- Um cliente Claude (qualquer um serve, pode ser os dois):
  - [Claude Desktop](https://claude.ai/download) — o aplicativo (recomendado pra quem não é dev)
  - [Claude Code](https://claude.ai/download) — a versão de terminal

> **Git é opcional.** Se você já usa git, pode clonar (veja o [guia técnico](INSTALL.md)). Senão, o Passo 3 baixa um ZIP direto — sem precisar instalar mais nada.

## Passo 2 — Pegue seu token do Pipedrive

1. Entre no seu Pipedrive
2. Clique na sua **foto de perfil** (canto superior direito)
3. Vá em **Configurações** > **Preferências pessoais** > **API**
4. Copie o **token da API pessoal** (uma sequência longa de letras e números)

Guarde esse token — você vai colar no Passo 4.

> **Cuidado:** esse token dá acesso ao SEU Pipedrive. Não compartilhe, não poste em grupo, não mande por e-mail. Cada pessoa deve pegar o próprio token.

## Passo 3 — Baixe o código

1. **Baixe o ZIP:** [pipedrive-mcp-main.zip](https://github.com/expertintegrado/pipedrive-mcp/archive/refs/heads/main.zip)
2. **Descompacte** numa pasta onde o MCP possa viver para sempre — tipo sua pasta de Documentos. **Não apague essa pasta depois** — é dela que o Claude vai rodar o MCP.
3. A pasta descompactada vai se chamar `pipedrive-mcp-main`. Renomeie para `pipedrive-mcp` se preferir (é opcional).
4. **Abra um terminal dentro dessa pasta** e rode:

   ```bash
   npm install
   ```

   (isso instala as dependências do Node.js — demora uns 30 segundos)

5. **Copie o caminho absoluto** da pasta — você vai precisar no Passo 4:

   ```bash
   # macOS/Linux
   pwd

   # Windows (PowerShell)
   (Get-Location).Path
   ```

   Vai aparecer algo como `/Users/seu-nome/Documents/pipedrive-mcp` ou `C:\Users\seu-nome\Documents\pipedrive-mcp`. Anote.

> **Prefere usar git?** `git clone https://github.com/expertintegrado/pipedrive-mcp.git && cd pipedrive-mcp && npm install`. Vantagem: atualizar depois é só `git pull` em vez de baixar o ZIP de novo.

## Passo 4 — Peça pro Claude configurar

Abra o Claude (Desktop ou Code) e **cole o texto abaixo**, trocando:

- `COLE_SEU_TOKEN_AQUI` pelo token do Passo 2
- `COLE_O_CAMINHO_ABSOLUTO_AQUI` pelo caminho do Passo 3 (até a pasta `pipedrive-mcp`)

> Configure o MCP do Pipedrive da Expert Integrado no meu cliente Claude. Já clonei o repositório em `COLE_O_CAMINHO_ABSOLUTO_AQUI` e já rodei `npm install` lá dentro. Adicione um servidor MCP chamado `pipedrive` que execute `node` com o argumento `COLE_O_CAMINHO_ABSOLUTO_AQUI/index.js` (use barras normais `/` mesmo no Windows, ou barras duplas `\\`). Configure as variáveis de ambiente `PIPEDRIVE_API_KEY=COLE_SEU_TOKEN_AQUI` e `PIPEDRIVE_TIMEZONE=America/Sao_Paulo`. **Importante:** se eu estiver no Claude Code, coloque no arquivo `.mcp.json` na raiz do projeto (não em `settings.json` — o Claude Code ignora MCPs dentro do `settings.json`). Se eu estiver no Claude Desktop, use o `claude_desktop_config.json` (Windows: `%APPDATA%\Claude\`, macOS: `~/Library/Application Support/Claude/`, Linux: `~/.config/Claude/`). Depois me explique o que você fez e me avise que preciso reiniciar o Claude.

O Claude vai:

1. Localizar/criar o arquivo de configuração certo
2. Adicionar o bloco `mcpServers` com o caminho e o token
3. Te avisar para reiniciar

**Feche e abra o Claude de novo** quando ele pedir.

> Prefere fazer manualmente? Siga o [guia técnico de instalação](INSTALL.md).

## Passo 5 — Teste

Pergunte ao Claude:

> Lista os meus deals abertos no Pipedrive.

Se ele responder com os deals, tá funcionando. 🎉

## Passo 6 — (Recomendado) Sincronize os dados da sua conta

Peça ao Claude, **uma vez só**:

> Execute o `sync_all` do Pipedrive.

Isso faz o MCP aprender os campos, pipelines, etapas e tipos de atividade da sua conta — respostas passam a usar nomes legíveis ao invés de números internos. Se você criar campos/pipelines novos lá no Pipedrive depois, repita esse comando.

---

## Atualizando o MCP

Quando sair versão nova:

**Se você baixou por ZIP:**

1. Baixe o [ZIP novo](https://github.com/expertintegrado/pipedrive-mcp/archive/refs/heads/main.zip)
2. Substitua o conteúdo da pasta antiga pelo conteúdo do ZIP novo (mantenha o **mesmo caminho** — senão você precisa reconfigurar o Claude)
3. Abra o terminal na pasta e rode `npm install` de novo

**Se você usou git:**

```bash
cd CAMINHO/pipedrive-mcp
git pull
npm install
```

Em ambos os casos, reinicie o Claude no fim. Se mudar algum campo esperado, rode `sync_all` de novo.

## Não funcionou? Peça ajuda pro Claude

Cole isso:

> O MCP do Pipedrive da Expert Integrado não está funcionando. Verifica meu arquivo de configuração do Claude (se for Claude Code, confira o `.mcp.json` na raiz do projeto — MCPs dentro de `settings.json` não funcionam), confere se o Node.js está instalado (versão 18+), se a pasta do repositório existe e tem `node_modules`, e me ajuda a resolver. Se precisar, consulte o guia em `https://github.com/expertintegrado/pipedrive-mcp/blob/main/docs/TROUBLESHOOTING.md`.

Se mesmo assim não rolar, [abra uma issue](https://github.com/expertintegrado/pipedrive-mcp/issues/new/choose) contando o que aconteceu.

## O que dá pra fazer

Exemplos de coisas que você pode pedir depois de instalado:

- *"Cria um deal chamado 'Empresa X - Plano Premium' pra pessoa Maria Silva"*
- *"Quais atividades eu tenho agendadas pra hoje?"*
- *"Marca uma ligação com o Pedro Santos pra amanhã às 14h"*
- *"Adiciona uma nota no deal 456 dizendo que o cliente pediu desconto de 10%"*
- *"Me mostra o histórico de movimentação do deal 789"*
- *"Busca todos os contatos que trabalham na empresa ABC"*

Lista completa de comandos: [docs/TOOLS.md](docs/TOOLS.md)

## Quer mudar seu fuso horário?

Por padrão usamos `America/Sao_Paulo`. Se você estiver em outro fuso, peça:

> No MCP do Pipedrive, troca a variável `PIPEDRIVE_TIMEZONE` pra `America/New_York` (ou o fuso que você usar).

## Instruções alternativas

Se você prefere instalar manualmente (sem passar pelo Claude) ou quer contribuir com o código, veja o [guia técnico de instalação](INSTALL.md).

## Segurança

- Seu token fica **apenas no seu computador**, dentro do arquivo de configuração do Claude
- Nenhum dado é enviado pra servidor externo — o MCP roda localmente
- Operações de exclusão são bloqueadas por padrão
- Campos já preenchidos são protegidos contra sobrescrita acidental

## Licença

[MIT](LICENSE) © Expert Integrado
