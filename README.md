# Pipedrive MCP

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![npm version](https://img.shields.io/npm/v/@expertintegrado/pipedrive-mcp.svg)](https://www.npmjs.com/package/@expertintegrado/pipedrive-mcp)

Conecta o seu **Pipedrive** ao **Claude Code**. Depois de instalar, você pede coisas como *"cria um deal pro cliente X"*, *"quais atividades eu tenho hoje?"*, *"adiciona uma nota no deal 123"* — e ele faz, direto no seu Pipedrive.

> A instalação é guiada pelo próprio Claude Code. Você só cola um prompt e passa seu token quando ele pedir. Não precisa editar arquivo, não precisa mexer em terminal.

---

## Passo 1 — Instale o que precisa

Baixe e instale (uma vez só, na sua máquina):

- [Node.js 18 ou superior](https://nodejs.org/) — baixe e clique "Avançar" até o fim. **Reinicie o computador** depois.
- [Claude Code](https://claude.ai/download) — o aplicativo oficial da Anthropic.

## Passo 2 — Pegue seu token do Pipedrive

1. Entre no seu Pipedrive
2. Clique na sua **foto de perfil** (canto superior direito)
3. Vá em **Configurações** > **Preferências pessoais** > **API**
4. Copie o **token da API pessoal** (uma sequência longa de letras e números)

Guarde esse token — o Claude vai pedir no próximo passo.

> **Cuidado:** esse token dá acesso ao SEU Pipedrive. Não compartilhe, não poste em grupo, não mande por e-mail. Cada pessoa deve pegar o próprio token.

## Passo 3 — Peça pro Claude Code instalar

Abra o **Claude Code** e cole o prompt abaixo no chat (use o botão de copiar no canto do bloco):

````text
Você é um instalador automático do MCP do Pipedrive da Expert Integrado.
Siga esta sequência:

1. Me pergunte no chat qual é o meu token da API do Pipedrive e
   aguarde minha resposta antes de continuar. Não use nenhum valor
   de exemplo — precisa ser o token real que eu vou colar.

2. Com o token em mãos, rode no terminal exatamente:

   claude mcp add pipedrive -s user \
     -e PIPEDRIVE_API_KEY=<TOKEN_QUE_EU_PASSEI> \
     -e PIPEDRIVE_TIMEZONE=America/Sao_Paulo \
     -- npx -y @expertintegrado/pipedrive-mcp

   Substituindo <TOKEN_QUE_EU_PASSEI> pelo token que eu respondi no
   passo 1.

3. Baixe e leia a documentação completa do MCP rodando:

   npm view @expertintegrado/pipedrive-mcp readme

   O README descreve o que o MCP faz, as ferramentas disponíveis,
   exemplos de uso e regras importantes. Use esse conteúdo como
   contexto pra me ajudar a usar o Pipedrive daqui pra frente —
   você não precisa me mostrar o README inteiro, só absorver
   internamente.

4. Confirme que tudo rodou sem erro e me avise pra encerrar e
   reabrir o Claude Code pra ativar o MCP.
````

O Claude Code vai:

1. Te perguntar o token → você cola o que pegou no Passo 2
2. Rodar o comando de configuração automaticamente
3. Baixar o README do pacote pra ter contexto completo do MCP
4. Te avisar pra reiniciar

Quando ele pedir, **feche e abra o Claude Code** (feche o app inteiro, não só a aba).

## Passo 4 — Teste

Com o Claude Code reaberto, pergunte:

> Lista os meus deals abertos no Pipedrive.

Se ele responder com os deals, tá funcionando. 🎉

## Passo 5 — (Recomendado) Sincronize os dados da sua conta

Peça ao Claude Code, **uma vez só**:

> Execute o `sync_all` do Pipedrive.

Isso faz o MCP aprender os campos, pipelines, etapas e tipos de atividade da sua conta — respostas passam a usar nomes legíveis ao invés de números internos. Se você criar campos/pipelines novos lá no Pipedrive depois, repita esse comando.

---

## Atualizando o MCP

Quando sair versão nova, o `npx` pega automaticamente na próxima inicialização — não precisa fazer nada. Se quiser forçar agora, peça ao Claude Code:

> Limpa o cache do npx do Pipedrive MCP (roda `npm cache clean --force`) e me avisa pra reiniciar o Claude Code.

Se a nova versão mudou campos ou pipelines esperados, rode `sync_all` de novo.

## Não funcionou?

Cole isso no Claude Code:

> O MCP do Pipedrive da Expert Integrado não está funcionando. Roda `/mcp` pra verificar se ele tá listado, confere se o Node.js 18+ está instalado, e me ajuda a diagnosticar. Se precisar, consulta o guia em `https://github.com/expertintegrado/pipedrive-mcp/blob/main/docs/TROUBLESHOOTING.md`.

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

Por padrão usamos `America/Sao_Paulo`. Se você estiver em outro fuso, peça ao Claude Code:

> No MCP do Pipedrive, troca a variável `PIPEDRIVE_TIMEZONE` pra `America/New_York` (ou o nome IANA do fuso que você usa) e reinicie.

## Instalação manual (fallback)

<details>
<summary>Se preferir configurar sem pedir pro Claude Code, clique aqui.</summary>

Rode no terminal (troque `SEU_TOKEN` pelo token do Passo 2):

```bash
claude mcp add pipedrive -s user \
  -e PIPEDRIVE_API_KEY=SEU_TOKEN \
  -e PIPEDRIVE_TIMEZONE=America/Sao_Paulo \
  -- npx -y @expertintegrado/pipedrive-mcp
```

Ou, se quiser editar o arquivo de configuração manualmente, adicione ao `~/.claude.json` (Claude Code, user scope) ou ao `.mcp.json` (Claude Code, por projeto):

```json
{
  "mcpServers": {
    "pipedrive": {
      "command": "npx",
      "args": ["-y", "@expertintegrado/pipedrive-mcp"],
      "env": {
        "PIPEDRIVE_API_KEY": "SEU_TOKEN",
        "PIPEDRIVE_TIMEZONE": "America/Sao_Paulo"
      }
    }
  }
}
```

Reinicie o Claude Code depois.

</details>

## Instalação alternativa (offline, Claude Desktop, contribuidor)

Se você:

- Está em ambiente **sem internet** (ou com proxy que bloqueia o npm registry)
- Quer usar o **Claude Desktop** (app de chat) em vez do Claude Code
- Vai **contribuir com o código** do MCP

→ Veja o [guia técnico de instalação](INSTALL.md) — tem os modos via ZIP download, `git clone`, e a configuração para Claude Desktop e outros clientes MCP.

## Segurança

- Seu token fica **apenas no seu computador**, dentro do arquivo de configuração do Claude
- Nenhum dado é enviado pra servidor externo — o MCP roda localmente na sua máquina
- Operações de exclusão são bloqueadas por padrão
- Campos já preenchidos são protegidos contra sobrescrita acidental

## Contribuindo

Quer reportar um bug, sugerir uma melhoria ou contribuir com código? Veja [CONTRIBUTING.md](CONTRIBUTING.md) e, para o procedimento de release, [RELEASING.md](RELEASING.md).

## Licença

[MIT](LICENSE) © Expert Integrado
