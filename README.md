# Pipedrive MCP

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Conecta o seu **Pipedrive** ao **Claude** (Claude Code ou Claude Desktop). Depois de instalar, voce pode pedir ao Claude coisas como *"cria um deal pro cliente X"*, *"quais atividades eu tenho hoje?"*, *"adiciona uma nota no deal 123"* — e ele faz, direto no seu Pipedrive.

> Voce nao precisa entender de programacao. Todo o processo de instalacao e feito **conversando com o Claude**. Siga o passo a passo abaixo.

---

## Passo 1 — Tenha o Claude instalado

Baixe um dos dois (qualquer um serve, pode ser os dois):

- [Claude Desktop](https://claude.ai/download) — o aplicativo (recomendado pra comecar)
- [Claude Code](https://claude.ai/download) — a versao que roda no terminal

E tambem instale o [Node.js 18 ou superior](https://nodejs.org/) (basta baixar e clicar "Avancar" ate o fim). Depois de instalar, **reinicie o computador**.

## Passo 2 — Pegue seu token do Pipedrive

1. Entre no seu Pipedrive
2. Clique na sua **foto de perfil** (canto superior direito)
3. Va em **Configuracoes** > **Preferencias pessoais** > **API**
4. Copie o **token da API pessoal** (uma sequencia longa de letras e numeros)

Guarde esse token — voce vai colar no Passo 3.

> **Cuidado:** esse token da acesso ao SEU Pipedrive. Nao compartilhe, nao poste em grupo, nao mande por e-mail. Cada pessoa deve pegar o proprio token.

## Passo 3 — Peca pro Claude instalar

Abra o Claude (Desktop ou Code) e **cole o texto abaixo**, trocando `COLE_SEU_TOKEN_AQUI` pelo token que voce copiou:

> Instale o MCP do Pipedrive da Expert Integrado pra mim. O repositorio e `https://github.com/expertintegrado/pipedrive-mcp`. Adicione no meu arquivo de configuracao do Claude (`claude_desktop_config.json` se eu estiver no Claude Desktop, ou `settings.json` se eu estiver no Claude Code) um servidor MCP chamado `pipedrive` usando `npx -y @expertintegrado/pipedrive-mcp` como comando. Configure as variaveis de ambiente `PIPEDRIVE_API_KEY=COLE_SEU_TOKEN_AQUI` e `PIPEDRIVE_TIMEZONE=America/Sao_Paulo`. Depois me explique o que voce fez e me avise se eu preciso reiniciar o Claude.

O Claude vai:
1. Encontrar o arquivo de configuracao certo pro seu sistema
2. Adicionar a integracao com o Pipedrive
3. Te avisar quando estiver pronto

**Feche e abra o Claude de novo** quando ele pedir.

## Passo 4 — Teste

Pergunte ao Claude:

> Lista os meus deals abertos no Pipedrive.

Se ele responder com os deals, ta funcionando. 🎉

## Passo 5 — (Recomendado) Sincronize os dados da sua conta

Peca ao Claude, **uma vez soh**:

> Execute o `sync_all` do Pipedrive.

Isso faz o MCP aprender os campos, pipelines, etapas e tipos de atividade da sua conta — respostas passam a usar nomes legiveis ao inves de numeros internos. Se voce criar campos/pipelines novos la no Pipedrive depois, repita esse comando.

---

## Nao funcionou? Peca ajuda pro Claude

Cole isso:

> O MCP do Pipedrive da Expert Integrado nao esta funcionando. Verifica meu arquivo de configuracao do Claude, confere se o Node.js esta instalado (versao 18+), e me ajuda a resolver. Se precisar, consulte o guia em `https://github.com/expertintegrado/pipedrive-mcp/blob/main/docs/TROUBLESHOOTING.md`.

Se mesmo assim nao rolar, [abra uma issue](https://github.com/expertintegrado/pipedrive-mcp/issues/new/choose) contando o que aconteceu.

## O que da pra fazer

Exemplos de coisas que voce pode pedir depois de instalado:

- *"Cria um deal chamado 'Empresa X - Plano Premium' pra pessoa Maria Silva"*
- *"Quais atividades eu tenho agendadas pra hoje?"*
- *"Marca uma ligacao com o Pedro Santos pra amanha as 14h"*
- *"Adiciona uma nota no deal 456 dizendo que o cliente pediu desconto de 10%"*
- *"Me mostra o historico de movimentacao do deal 789"*
- *"Busca todos os contatos que trabalham na empresa ABC"*

Lista completa de comandos: [docs/TOOLS.md](docs/TOOLS.md)

## Quer mudar seu fuso horario?

Por padrao usamos `America/Sao_Paulo`. Se voce estiver em outro fuso, peca:

> No MCP do Pipedrive, troca a variavel `PIPEDRIVE_TIMEZONE` pra `America/New_York` (ou o fuso que voce usar).

## Instrucoes alternativas

Se voce prefere instalar manualmente (sem passar pelo Claude) ou quer contribuir com o codigo, veja o [guia tecnico de instalacao](INSTALL.md).

## Seguranca

- Seu token fica **apenas no seu computador**, dentro do arquivo de configuracao do Claude
- Nenhum dado e enviado pra servidor externo — o MCP roda localmente
- Operacoes de exclusao sao bloqueadas por padrao
- Campos ja preenchidos sao protegidos contra sobrescrita acidental

## Licenca

[MIT](LICENSE) © Expert Integrado
