# Sincronizacao — `sync_all` e `config.js`

## O que e o `config.js`

Arquivo unico que contem todos os dados de referencia da sua conta Pipedrive:

- Campos customizados de deals
- Campos customizados de contatos (persons)
- Tipos de atividade (com aliases e duracoes padrao)
- Pipelines e etapas
- Usuarios ativos
- Dominio da empresa (ex: `expertintegrado` em `expertintegrado.pipedrive.com`)

O MCP carrega esse arquivo no startup e usa como cache — evita chamadas repetidas a API e permite resolucao por nome (ex: `pipeline: "Vendas"` em vez de `pipeline_id: 3`).

## Quando rodar `sync_all`

Na **primeira instalacao** e sempre que algo mudar no Pipedrive:

- Criou ou renomeou um campo customizado
- Adicionou/removeu um tipo de atividade
- Criou um novo pipeline ou etapa
- Contratou ou desativou um usuario

Peca ao Claude:

> Execute `sync_all` do Pipedrive.

O arquivo `config.js` e gerado/sobrescrito na pasta onde o servidor MCP esta rodando.

## Aliases e duracoes padrao

Depois de sincronizar, voce pode editar o `config.js` para adicionar:

- **Aliases** por tipo de atividade (ex: `"ligacao"` como alias de `"call"`)
- **Duracao padrao** em minutos (ex: reuniao = 60min)

Ao sincronizar de novo, o `sync_all` **preserva aliases e duracoes** existentes.

## Distribuicao para equipe (sem permissao admin)

Se membros da equipe nao tem permissao de admin no Pipedrive para rodar `sync_all`:

1. O admin executa `sync_all` na maquina dele
2. Copia o `config.js` gerado
3. Distribui para os colegas
4. Cada colega coloca o `config.js` na pasta do MCP

> Se o MCP foi instalado via `npx` (Modo 1), a pasta de trabalho costuma ser a pasta atual quando o cliente MCP inicia — no Claude Desktop, geralmente `%USERPROFILE%` no Windows ou `~` no macOS. Peca ao Claude: *"onde o `config.js` foi salvo?"* — ele sabe.

## Fallback

Se o `config.js` nao existir ou estiver incompleto, o MCP tenta recarregar via API no startup. Se a API tambem falhar, algumas respostas retornam IDs numericos em vez de nomes legiveis — rode `sync_all` para corrigir.

## Seguranca

O `config.js` contem **metadados da sua conta** (nomes de campos, pipelines, usuarios — nao contem dados de deals/contatos). Ainda assim, esta no `.gitignore` por padrao. **Nao commite** em repositorios publicos.
