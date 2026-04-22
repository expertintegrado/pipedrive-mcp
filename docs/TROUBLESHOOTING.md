# Troubleshooting

## O MCP nao aparece no Claude

1. Confira que o JSON em `settings.json` / `claude_desktop_config.json` esta valido (sem virgula sobrando, aspas corretas).
2. Reinicie completamente o Claude Code / Claude Desktop.
3. No Claude Desktop, abra **Developer > Open MCP Log File** para ver erros de inicializacao.
4. No Claude Code, rode `/mcp` — deve listar `pipedrive` como conectado.

## Erro: `PIPEDRIVE_API_KEY not set`

O token nao chegou como variavel de ambiente. Verifique se o bloco `env` esta dentro de `pipedrive` (nao no nivel raiz do JSON) e se o cliente foi reiniciado apos salvar.

## Erro: `401 Unauthorized` ou `403 Forbidden`

- Token expirado ou revogado. Gere outro em **Pipedrive > Configuracoes > Dados pessoais > API**.
- Token de conta sem permissao para a operacao (ex: usuario nao-admin tentando criar campos).

## Respostas mostram IDs em vez de nomes

O `config.js` nao foi carregado. Rode `sync_all` — veja [SYNC.md](SYNC.md).

## Atividade criada em horario errado (+/- 3h)

Verifique `PIPEDRIVE_TIMEZONE`. O padrao e `America/Sao_Paulo`. Se voce esta em outro fuso, ajuste:

```json
"env": {
  "PIPEDRIVE_API_KEY": "...",
  "PIPEDRIVE_TIMEZONE": "America/New_York"
}
```

Use o [nome IANA](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) do fuso.

## `sync_all` falha com timeout

Contas com muitos usuarios/campos podem estourar o tempo. Tente novamente. Se persistir, abra uma [issue](https://github.com/expertintegrado/pipedrive-mcp/issues) com o erro completo.

## Mudei algo no Pipedrive e o MCP nao reconhece

Rode `sync_all` de novo para regenerar o `config.js`. Depois reinicie o cliente MCP (o arquivo e lido no startup).

## `npx` baixa o pacote toda vez — posso evitar?

Sim, troque pelo Modo 2 ([INSTALL.md](../INSTALL.md#modo-2--npm-install--g)): `npm install -g @expertintegrado/pipedrive-mcp` e use `"command": "pipedrive-mcp"`.

## Links das respostas apontam para o dominio errado

O dominio e detectado automaticamente via `/users/me` no startup. Se esta errado:
1. Confirme que o token pertence a conta certa.
2. Rode `sync_all` para atualizar `config.js`.
3. Reinicie o cliente.

## Nao encontrou a resposta?

[Abra uma issue](https://github.com/expertintegrado/pipedrive-mcp/issues) descrevendo o problema, a versao do Node (`node --version`) e qual cliente MCP voce usa.
