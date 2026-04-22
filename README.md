# Pipedrive MCP

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![npm version](https://img.shields.io/npm/v/@expertintegrado/pipedrive-mcp.svg)](https://www.npmjs.com/package/@expertintegrado/pipedrive-mcp)

Conecta o seu **Pipedrive** ao **Claude Desktop**. Depois de instalar, você pede coisas como *"cria um deal pro cliente X"*, *"quais atividades eu tenho hoje?"*, *"adiciona uma nota no deal 123"* — e ele faz, direto no seu Pipedrive.

> Instalação em 3 passos: instalar Node.js, pegar seu token do Pipedrive e colar um bloco de configuração. Não precisa baixar código, nem clonar repositório, nem mexer em terminal.

---

## Passo 1 — Instale o que precisa

Baixe e instale (uma vez só, na sua máquina):

- [Node.js 18 ou superior](https://nodejs.org/) — baixe e clique "Avançar" até o fim. **Reinicie o computador** depois.
- [Claude Desktop](https://claude.ai/download) — o aplicativo oficial da Anthropic.

## Passo 2 — Pegue seu token do Pipedrive

1. Entre no seu Pipedrive
2. Clique na sua **foto de perfil** (canto superior direito)
3. Vá em **Configurações** > **Preferências pessoais** > **API**
4. Copie o **token da API pessoal** (uma sequência longa de letras e números)

Guarde esse token — você vai colar no próximo passo.

> **Cuidado:** esse token dá acesso ao SEU Pipedrive. Não compartilhe, não poste em grupo, não mande por e-mail. Cada pessoa deve pegar o próprio token.

## Passo 3 — Configure o MCP no Claude Desktop

1. Abra o **Claude Desktop**
2. Vá em **Settings** (Configurações) → **Developer** → clique em **Edit Config**
3. Isso abre o arquivo `claude_desktop_config.json`. Cole o bloco abaixo (se já tiver outros MCPs, adicione apenas o conteúdo de dentro de `mcpServers`):

   ```json
   {
     "mcpServers": {
       "pipedrive": {
         "command": "npx",
         "args": ["-y", "@expertintegrado/pipedrive-mcp"],
         "env": {
           "PIPEDRIVE_API_KEY": "COLE_SEU_TOKEN_AQUI",
           "PIPEDRIVE_TIMEZONE": "America/Sao_Paulo"
         }
       }
     }
   }
   ```

4. Troque `COLE_SEU_TOKEN_AQUI` pelo token do Passo 2
5. Salve o arquivo
6. **Feche e abra o Claude Desktop** (não basta fechar a aba — feche o app inteiro)

Na primeira vez que você abrir, o `npx` vai baixar o pacote automaticamente (uns 10–30 segundos). Depois disso, inicia em segundos.

> **Não sabe onde fica o `claude_desktop_config.json`?** O botão "Edit Config" abre pra você. Se não encontrar: Windows `%APPDATA%\Claude\claude_desktop_config.json` · macOS `~/Library/Application Support/Claude/claude_desktop_config.json` · Linux `~/.config/Claude/claude_desktop_config.json`.

## Passo 4 — Teste

Com o Claude Desktop reaberto, pergunte:

> Lista os meus deals abertos no Pipedrive.

Se ele responder com os deals, tá funcionando. 🎉

## Passo 5 — (Recomendado) Sincronize os dados da sua conta

Peça ao Claude Desktop, **uma vez só**:

> Execute o `sync_all` do Pipedrive.

Isso faz o MCP aprender os campos, pipelines, etapas e tipos de atividade da sua conta — respostas passam a usar nomes legíveis ao invés de números internos. Se você criar campos/pipelines novos lá no Pipedrive depois, repita esse comando.

---

## Atualizando o MCP

Quando sair versão nova, o `npx` pega automaticamente na próxima inicialização do Claude Desktop — não precisa fazer nada. Se quiser forçar a atualização agora, peça ao Claude:

> Limpa o cache do npx do Pipedrive MCP pra pegar a versão mais nova.

Se alguma versão mudou campos ou pipelines esperados, rode `sync_all` de novo.

## Não funcionou?

Cole isso no Claude Desktop:

> O MCP do Pipedrive da Expert Integrado não está funcionando. Verifica se está listado nos MCPs conectados, confere se o Node.js 18+ está instalado e me ajuda a resolver. Se precisar, consulta o guia em `https://github.com/expertintegrado/pipedrive-mcp/blob/main/docs/TROUBLESHOOTING.md`.

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

Por padrão usamos `America/Sao_Paulo`. Se você estiver em outro fuso, edite a variável `PIPEDRIVE_TIMEZONE` no bloco JSON do Passo 3 — use o [nome IANA](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) do fuso (ex: `America/New_York`, `Europe/Lisbon`). Salve e reinicie o Claude Desktop.

## Instalação alternativa (offline, Claude Code, contribuidor)

Se você:

- Está em ambiente **sem internet** (ou com proxy que bloqueia o npm registry)
- Quer usar o **Claude Code** (CLI ou projetos com `.mcp.json`)
- Vai **contribuir com o código** do MCP

→ Veja o [guia técnico de instalação](INSTALL.md) — tem os modos via ZIP download e `git clone`, e a configuração específica pro Claude Code.

## Segurança

- Seu token fica **apenas no seu computador**, dentro do arquivo de configuração do Claude
- Nenhum dado é enviado pra servidor externo — o MCP roda localmente na sua máquina
- Operações de exclusão são bloqueadas por padrão
- Campos já preenchidos são protegidos contra sobrescrita acidental

## Licença

[MIT](LICENSE) © Expert Integrado
