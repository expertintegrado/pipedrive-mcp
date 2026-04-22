# Pipedrive MCP

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Conecta o seu **Pipedrive** ao **Claude Code**. Depois de instalar, você pede coisas como *"cria um deal pro cliente X"*, *"quais atividades eu tenho hoje?"*, *"adiciona uma nota no deal 123"* — e ele faz, direto no seu Pipedrive.

> Você **não precisa** rodar nenhum comando manual. O próprio Claude Code cuida da instalação — você só baixa o código, abre ele nessa pasta, e cola um prompt.

---

## Passo 1 — Instale o que precisa

Baixe e instale (uma vez só, na sua máquina):

- [Node.js 18 ou superior](https://nodejs.org/) — baixe e clique "Avançar" até o fim. **Reinicie o computador** depois.
- [Claude Code](https://claude.ai/download)

## Passo 2 — Pegue seu token do Pipedrive

1. Entre no seu Pipedrive
2. Clique na sua **foto de perfil** (canto superior direito)
3. Vá em **Configurações** > **Preferências pessoais** > **API**
4. Copie o **token da API pessoal** (uma sequência longa de letras e números)

Guarde esse token — você vai colar no Passo 5.

> **Cuidado:** esse token dá acesso ao SEU Pipedrive. Não compartilhe, não poste em grupo, não mande por e-mail. Cada pessoa deve pegar o próprio token.

## Passo 3 — Baixe o código

1. Clique aqui para baixar o ZIP: **[pipedrive-mcp-main.zip](https://github.com/expertintegrado/pipedrive-mcp/archive/refs/heads/main.zip)**
2. Descompacte numa pasta que não será apagada nunca — tipo sua pasta de **Documentos**. É dessa pasta que o Claude Code vai rodar o MCP pra sempre.
3. A pasta descompactada se chama `pipedrive-mcp-main`. Pode renomear pra `pipedrive-mcp` se preferir (é opcional).

## Passo 4 — Abra o Claude Code nessa pasta

**Windows:**

- Abra a pasta que você descompactou no Windows Explorer
- Clique com **botão direito** dentro dela → **"Abrir no Terminal"** (ou "Abrir janela do PowerShell aqui")
- No terminal que abrir, digite:
  ```
  claude
  ```
  e tecle Enter.

**macOS:**

- Abra o **Terminal** (Cmd+Espaço → digite "Terminal" → Enter)
- Digite `cd ` (com espaço no fim), **arraste a pasta descompactada** pra dentro do terminal e tecle Enter
- Agora digite:
  ```
  claude
  ```
  e tecle Enter.

**Linux:**

- Abra o terminal dentro da pasta descompactada
- Digite `claude` e tecle Enter.

## Passo 5 — Peça pro Claude Code instalar

Quando o Claude Code abrir, **cole este prompt**, trocando `COLE_SEU_TOKEN_AQUI` pelo token do Passo 2:

> Sou um usuário novo desse MCP e estou com a pasta do projeto aberta aqui. Faça a instalação completa pra mim: rode `npm install` nesta pasta pra baixar as dependências, descubra o caminho absoluto do `index.js` desta pasta, e configure o MCP do Pipedrive usando `claude mcp add` com escopo de usuário (`-s user`) pra funcionar em todos os meus projetos. O comando do MCP é `node` com o caminho absoluto do `index.js` como argumento. Use as variáveis de ambiente `PIPEDRIVE_API_KEY=COLE_SEU_TOKEN_AQUI` e `PIPEDRIVE_TIMEZONE=America/Sao_Paulo`. No final, confirme o que foi feito e me avise pra eu encerrar e abrir o Claude Code de novo.

O Claude Code vai:

1. Instalar as dependências (`npm install`)
2. Descobrir o caminho da pasta automaticamente
3. Configurar o MCP pra funcionar em qualquer projeto seu
4. Te avisar pra reiniciar

Quando ele pedir, **encerre o Claude Code** (`Ctrl+D` ou feche a janela) e abra de novo.

## Passo 6 — Teste

Com o Claude Code reaberto, pergunte:

> Lista os meus deals abertos no Pipedrive.

Se ele responder com os deals, tá funcionando. 🎉

## Passo 7 — (Recomendado) Sincronize os dados da sua conta

Peça ao Claude Code, **uma vez só**:

> Execute o `sync_all` do Pipedrive.

Isso faz o MCP aprender os campos, pipelines, etapas e tipos de atividade da sua conta — respostas passam a usar nomes legíveis ao invés de números internos. Se você criar campos/pipelines novos lá no Pipedrive depois, repita esse comando.

---

## Atualizando o MCP

Quando sair versão nova, abra o Claude Code na pasta do MCP e peça:

> Atualiza o MCP do Pipedrive pra última versão. Baixa o código novo do GitHub, substitui o conteúdo desta pasta e roda `npm install` de novo.

Depois encerre e abra o Claude Code novamente. Se alguma coisa mudou nos campos do Pipedrive, rode `sync_all` de novo.

## Não funcionou?

Cole isso no Claude Code:

> O MCP do Pipedrive da Expert Integrado não está funcionando. Verifica se o MCP está listado (rode `/mcp`), confere se o Node.js 18+ está instalado, se a pasta do repositório tem `node_modules` dentro, e me ajuda a resolver. Se precisar, consulta o guia em `https://github.com/expertintegrado/pipedrive-mcp/blob/main/docs/TROUBLESHOOTING.md`.

Se mesmo assim não rolar, [abra uma issue](https://github.com/expertintegrado/pipedrive-mcp/issues/new/choose) contando o que aconteceu.

## O que dá pra fazer

Exemplos depois de instalado:

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

## Instalação alternativa (manual, Claude Desktop, contribuidor)

Se você:

- Usa **Claude Desktop** (o aplicativo de chat, não o Claude Code)
- Prefere editar os arquivos de configuração **na mão** (sem pedir pro Claude Code)
- Quer clonar com **git** em vez de baixar ZIP (pra atualizar com `git pull`)
- Vai **contribuir com o código**

→ Siga o [guia técnico de instalação](INSTALL.md).

## Segurança

- Seu token fica **apenas no seu computador**, dentro do arquivo de configuração do Claude Code
- Nenhum dado é enviado pra servidor externo — o MCP roda localmente
- Operações de exclusão são bloqueadas por padrão
- Campos já preenchidos são protegidos contra sobrescrita acidental

## Licença

[MIT](LICENSE) © Expert Integrado
