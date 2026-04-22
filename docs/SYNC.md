# Sincronização — `sync_all` e `config.js`

## O que é o `config.js`

Arquivo único que contém todos os dados de referência da sua conta Pipedrive:

- Campos customizados de deals
- Campos customizados de contatos (persons)
- Tipos de atividade (com aliases e durações padrão)
- Pipelines e etapas
- Usuários ativos
- Domínio da empresa (ex: `expertintegrado` em `expertintegrado.pipedrive.com`)

O MCP carrega esse arquivo no startup e usa como cache — evita chamadas repetidas à API e permite resolução por nome (ex: `pipeline: "Vendas"` em vez de `pipeline_id: 3`).

## Quando rodar `sync_all`

Na **primeira instalação** e sempre que algo mudar no Pipedrive:

- Criou ou renomeou um campo customizado
- Adicionou/removeu um tipo de atividade
- Criou um novo pipeline ou etapa
- Contratou ou desativou um usuário

Peça ao Claude:

> Execute `sync_all` do Pipedrive.

O arquivo `config.js` é gerado/sobrescrito na pasta onde o servidor MCP está rodando.

## Aliases e durações padrão

Depois de sincronizar, você pode editar o `config.js` para adicionar:

- **Aliases** por tipo de atividade (ex: `"ligação"` como alias de `"call"`)
- **Duração padrão** em minutos (ex: reunião = 60min)

Ao sincronizar de novo, o `sync_all` **preserva aliases e durações** existentes.

## Distribuição para equipe (sem permissão admin)

Se membros da equipe não têm permissão de admin no Pipedrive para rodar `sync_all`:

1. O admin executa `sync_all` na máquina dele
2. Copia o `config.js` gerado
3. Distribui para os colegas
4. Cada colega coloca o `config.js` na pasta do MCP

> Onde o `config.js` é salvo depende do modo de instalação:
> - **Modo 1 (npx)** — na pasta de trabalho do cliente MCP, normalmente `%USERPROFILE%` no Windows ou `~` no macOS/Linux.
> - **Modos 2/3 (local)** — dentro da pasta do repositório clonado.
>
> Na dúvida, pergunte ao Claude: *"onde o `config.js` foi salvo?"* — ele sabe.

## Fallback

Se o `config.js` não existir ou estiver incompleto, o MCP tenta recarregar via API no startup. Se a API também falhar, algumas respostas retornam IDs numéricos em vez de nomes legíveis — rode `sync_all` para corrigir.

## Segurança

O `config.js` contém **metadados da sua conta** (nomes de campos, pipelines, usuários — não contém dados de deals/contatos). Ainda assim, está no `.gitignore` por padrão. **Não commite** em repositórios públicos.
