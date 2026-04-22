# Pipedrive MCP

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Conecta o seu **Pipedrive** ao **Claude** (Claude Code ou Claude Desktop). Depois de instalar, você pode pedir ao Claude coisas como *"cria um deal pro cliente X"*, *"quais atividades eu tenho hoje?"*, *"adiciona uma nota no deal 123"* — e ele faz, direto no seu Pipedrive.

> Você não precisa entender de programação. Todo o processo de instalação é feito **conversando com o Claude**. Siga o passo a passo abaixo.

---

## Passo 1 — Tenha o Claude instalado

Baixe um dos dois (qualquer um serve, pode ser os dois):

- [Claude Desktop](https://claude.ai/download) — o aplicativo (recomendado pra começar)
- [Claude Code](https://claude.ai/download) — a versão que roda no terminal

E também instale o [Node.js 18 ou superior](https://nodejs.org/) (basta baixar e clicar "Avançar" até o fim). Depois de instalar, **reinicie o computador**.

## Passo 2 — Pegue seu token do Pipedrive

1. Entre no seu Pipedrive
2. Clique na sua **foto de perfil** (canto superior direito)
3. Vá em **Configurações** > **Preferências pessoais** > **API**
4. Copie o **token da API pessoal** (uma sequência longa de letras e números)

Guarde esse token — você vai colar no Passo 3.

> **Cuidado:** esse token dá acesso ao SEU Pipedrive. Não compartilhe, não poste em grupo, não mande por e-mail. Cada pessoa deve pegar o próprio token.

## Passo 3 — Peça pro Claude instalar

Abra o Claude (Desktop ou Code) e **cole o texto abaixo**, trocando `COLE_SEU_TOKEN_AQUI` pelo token que você copiou:

> Instale o MCP do Pipedrive da Expert Integrado pra mim. O repositório é `https://github.com/expertintegrado/pipedrive-mcp`. Adicione no meu arquivo de configuração do Claude (`claude_desktop_config.json` se eu estiver no Claude Desktop, ou `settings.json` se eu estiver no Claude Code) um servidor MCP chamado `pipedrive` usando `npx -y @expertintegrado/pipedrive-mcp` como comando. Configure as variáveis de ambiente `PIPEDRIVE_API_KEY=COLE_SEU_TOKEN_AQUI` e `PIPEDRIVE_TIMEZONE=America/Sao_Paulo`. Depois me explique o que você fez e me avise se eu preciso reiniciar o Claude.

O Claude vai:
1. Encontrar o arquivo de configuração certo pro seu sistema
2. Adicionar a integração com o Pipedrive
3. Te avisar quando estiver pronto

**Feche e abra o Claude de novo** quando ele pedir.

## Passo 4 — Teste

Pergunte ao Claude:

> Lista os meus deals abertos no Pipedrive.

Se ele responder com os deals, tá funcionando. 🎉

## Passo 5 — (Recomendado) Sincronize os dados da sua conta

Peça ao Claude, **uma vez só**:

> Execute o `sync_all` do Pipedrive.

Isso faz o MCP aprender os campos, pipelines, etapas e tipos de atividade da sua conta — respostas passam a usar nomes legíveis ao invés de números internos. Se você criar campos/pipelines novos lá no Pipedrive depois, repita esse comando.

---

## Não funcionou? Peça ajuda pro Claude

Cole isso:

> O MCP do Pipedrive da Expert Integrado não está funcionando. Verifica meu arquivo de configuração do Claude, confere se o Node.js está instalado (versão 18+), e me ajuda a resolver. Se precisar, consulte o guia em `https://github.com/expertintegrado/pipedrive-mcp/blob/main/docs/TROUBLESHOOTING.md`.

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
