# Troubleshooting

## O MCP não aparece no Claude

1. Confira que o JSON em `settings.json` / `claude_desktop_config.json` está válido (sem vírgula sobrando, aspas corretas).
2. Reinicie completamente o Claude Code / Claude Desktop.
3. No Claude Desktop, abra **Developer > Open MCP Log File** para ver erros de inicialização.
4. No Claude Code, rode `/mcp` — deve listar `pipedrive` como conectado.

## Erro: `PIPEDRIVE_API_KEY not set`

O token não chegou como variável de ambiente. Verifique se o bloco `env` está dentro de `pipedrive` (não no nível raiz do JSON) e se o cliente foi reiniciado após salvar.

## Erro: `401 Unauthorized` ou `403 Forbidden`

- Token expirado ou revogado. Gere outro em **Pipedrive > Configurações > Dados pessoais > API**.
- Token de conta sem permissão para a operação (ex: usuário não-admin tentando criar campos).

## Respostas mostram IDs em vez de nomes

O `config.js` não foi carregado. Rode `sync_all` — veja [SYNC.md](SYNC.md).

## Atividade criada em horário errado (+/- 3h)

Verifique `PIPEDRIVE_TIMEZONE`. O padrão é `America/Sao_Paulo`. Se você está em outro fuso, ajuste:

```json
"env": {
  "PIPEDRIVE_API_KEY": "...",
  "PIPEDRIVE_TIMEZONE": "America/New_York"
}
```

Use o [nome IANA](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) do fuso.

## `sync_all` falha com timeout

Contas com muitos usuários/campos podem estourar o tempo. Tente novamente. Se persistir, abra uma [issue](https://github.com/expertintegrado/pipedrive-mcp/issues) com o erro completo.

## Mudei algo no Pipedrive e o MCP não reconhece

Rode `sync_all` de novo para regenerar o `config.js`. Depois reinicie o cliente MCP (o arquivo é lido no startup).

## `npx` baixa o pacote toda vez — posso evitar?

Sim, troque pelo Modo 2 ([INSTALL.md](../INSTALL.md#modo-2--npm-install--g)): `npm install -g @expertintegrado/pipedrive-mcp` e use `"command": "pipedrive-mcp"`.

## Links das respostas apontam para o domínio errado

O domínio é detectado automaticamente via `/users/me` no startup. Se está errado:
1. Confirme que o token pertence à conta certa.
2. Rode `sync_all` para atualizar `config.js`.
3. Reinicie o cliente.

## Não encontrou a resposta?

[Abra uma issue](https://github.com/expertintegrado/pipedrive-mcp/issues) descrevendo o problema, a versão do Node (`node --version`) e qual cliente MCP você usa.
