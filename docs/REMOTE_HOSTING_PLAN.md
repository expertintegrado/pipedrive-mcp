# Plano — Hospedagem remota do Pipedrive MCP

Documento de planejamento para migrar o MCP de execução local (stdio) para um servidor remoto (HTTP streamable), eliminando a necessidade de o mentorado instalar Node.js e rodar qualquer coisa localmente.

**Status:** em planejamento — não iniciado.
**Última atualização:** 2026-04-22.

---

## Objetivo

Permitir que um mentorado configure o MCP no Claude apenas colando uma **URL** no `claude_desktop_config.json`, sem instalar Node, sem `npx`, sem clonar repositório.

Hoje:
```json
{ "mcpServers": { "pipedrive": { "command": "npx", "args": ["-y", "@expertintegrado/pipedrive-mcp"], "env": { "PIPEDRIVE_API_KEY": "..." } } } }
```

Futuro:
```json
{ "mcpServers": { "pipedrive": { "url": "https://pipedrive-mcp.expertintegrado.workers.dev/mcp" } } }
```

---

## Mudanças técnicas necessárias

### 1. Transport: stdio → streamable-http

O MCP SDK já suporta os dois. Hoje usamos:

```js
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
```

No remoto, trocamos por `StreamableHTTPServerTransport` e expomos em uma rota HTTP (`POST /mcp` tipicamente).

**Impacto no código:** pequeno no núcleo. Os handlers de tools (`server.tool(...)`) continuam iguais. Muda só o bootstrap.

### 2. Persistência: `fs` → armazenamento gerenciado

O código atual escreve `config.js` em disco via `fs.writeFileSync`. Em ambientes serverless (Cloudflare Workers, Vercel) **não há filesystem persistente**.

**Solução:** trocar `fs` por:

- **Cloudflare KV** (chave-valor simples) — ideal para o `config.js` que é lido inteiro no startup
- **Cloudflare D1** (SQLite) — se quisermos consultas mais complexas
- **Durable Objects** — se precisarmos de estado consistente por usuário

**Impacto no código:** médio. Toda leitura/escrita de `config.js` precisa virar async e passar por uma camada de storage. Ficam ~15-20 pontos de alteração no `index.js`.

### 3. Multi-tenancy do token Pipedrive

Hoje o token vem numa `env`, processo local, isolado por design. Remoto, o servidor atende vários usuários — **cada request precisa carregar o token do usuário certo**.

Três caminhos, em ordem de complexidade:

---

## Caminhos de autenticação

### Caminho A — Token por header (MVP)

O mentorado passa o próprio token em cada request via header HTTP.

**Config do Claude:**
```json
{
  "mcpServers": {
    "pipedrive": {
      "url": "https://pipedrive-mcp.expertintegrado.workers.dev/mcp",
      "headers": {
        "X-Pipedrive-Token": "token_do_usuario_aqui"
      }
    }
  }
}
```

**Como funciona:** a cada request, o servidor lê o header, usa o token para chamar a API do Pipedrive, não armazena nada.

**Prós:**
- Implementação muito simples (1-2 dias)
- Servidor stateless — sem banco, sem sessão
- Mentorado não precisa criar conta em lugar nenhum

**Contras:**
- Token trafega em todo request (TLS mitiga, mas é uma superfície)
- Mentorado ainda precisa editar arquivo JSON manualmente (mesma UX de hoje, só troca `env` por `headers`)
- Sem `config.js` persistente — ou re-sincroniza toda vez, ou armazena em KV com chave derivada do hash do token

### Caminho B — Setup page + session ID (meio-termo)

O mentorado acessa uma página web uma única vez, cola o token, recebe uma URL personalizada.

**Fluxo:**
1. Mentorado abre `https://pipedrive-mcp.expertintegrado.workers.dev/setup`
2. Cola o token Pipedrive
3. Servidor valida o token contra a API do Pipedrive, gera um `session_id` (UUID), grava no KV: `session:uuid -> { token, config_js, created_at }`
4. Página retorna a URL final:
   ```
   https://pipedrive-mcp.expertintegrado.workers.dev/mcp/abc-123-uuid
   ```
5. Mentorado cola essa URL no Claude

**Prós:**
- Token não trafega em cada request (fica no servidor)
- `config.js` persiste por sessão
- Mentorado não precisa mexer em JSON complicado — só cola uma URL
- Pode-se revogar sessão sem trocar o token Pipedrive

**Contras:**
- Precisa página de setup (HTML/CSS/JS simples no próprio Worker)
- Mentorado perde a URL = perde acesso (precisa rotina de recuperação ou reset via email)
- Precisa gerenciar tempo de vida da sessão, rotação, revogação

### Caminho C — OAuth com Pipedrive (robusto)

Fluxo OAuth 2.0 completo: mentorado clica "Conectar Pipedrive", autoriza na tela oficial do Pipedrive, servidor recebe refresh token e renova sozinho.

**Pré-requisitos:**
- Registrar um **app OAuth oficial no Pipedrive Marketplace** (requer conta Pipedrive Professional+ e submissão para revisão do Pipedrive, que pode demorar dias a semanas)
- Hostname fixo com HTTPS (callback URL precisa ser pré-registrada)

**Prós:**
- UX ideal: um clique, sem token visível
- Renovação automática de credenciais
- Permissões granulares (OAuth scopes)
- Credenciais revogáveis pelo mentorado no painel do Pipedrive

**Contras:**
- Implementação complexa (1-2 semanas)
- Depende da aprovação do app no Pipedrive
- Custo operacional maior (logs de auth, gestão de refresh tokens)

---

## Recomendação de plataforma: Cloudflare Workers

**Por quê:**
- Único provedor com suporte **first-class a MCP remoto** (SDK `agents/mcp` publicado por Cloudflare, documentado pela Anthropic)
- Free tier cobre milhares de requests/dia
- KV, D1 e Durable Objects integrados — resolve persistência nativamente
- Deploy por `wrangler deploy` (1 comando)
- HTTPS automático em `*.workers.dev`
- Domínio customizado fácil (ex: `mcp.expertintegrado.com.br`)

**Alternativas consideradas:**

| Plataforma | Avaliação |
|---|---|
| **Railway / Fly.io / Render** | Rodam Node puro, zero refactor. Mas você gerencia container, logs, escala. ~$5-10/mês. Bom plano B. |
| **Vercel** | Funciona para stateless. Limite de execução curto pode atrapalhar `sync_all`. Já temos conta, vale testar. |
| **AWS Lambda** | Funciona, mas complexidade de setup alta demais pra esse caso. |

---

## Estimativas de esforço

| Escopo | Dias de codificação |
|---|---|
| **Caminho A (token por header)** — Workers + KV opcional | 1-2 dias |
| **Caminho B (setup page + session)** — Workers + KV + página HTML | 4-6 dias |
| **Caminho C (OAuth Pipedrive)** — Workers + D1 + registro de app | 2-3 semanas (incluindo espera de aprovação do Pipedrive) |
| Testes, logs, rate limit, monitoramento (em cima de qualquer caminho) | +3-5 dias |

## Custo operacional estimado

**Cloudflare Workers Free tier:** 100k requests/dia, 10ms CPU/request. Cobre com folga até ~100 mentorados ativos.

**Workers Paid ($5/mês):** 10M requests/mês. Cobre ~1.000 mentorados.

**KV:** 100k leituras/dia gratuitas. Suficiente para qualquer fase inicial.

**Custo total inicial previsto:** $0-5/mês.

---

## Riscos e pontos de atenção

1. **Rate limits da API do Pipedrive** — cada plano tem limites (Professional = 100 req/2s por token). Se 10 mentorados da mesma conta usarem ao mesmo tempo, um pode bloquear o outro. **Mitigação:** backoff exponencial + fila por token no servidor.

2. **Segurança do token no servidor (Caminhos B e C)** — precisamos criptografar em repouso. Cloudflare KV não criptografa by default — usar `crypto.subtle` para AES-256-GCM com chave em `wrangler secret`.

3. **Latência** — chamada local é instantânea; remota passa pelo Cloudflare edge + API Pipedrive. Esperamos +100-300ms por tool call. Aceitável para uso conversacional.

4. **Debugging** — sem `console.log` local, precisamos Cloudflare Logs + alerta de erro (pode integrar com Sentry ou só usar `wrangler tail`).

5. **Quebras de conexão** — streamable-http mantém conexão aberta por tool call. Se o Worker hibernar (após ~10s), a conexão cai. **Mitigação:** Durable Objects para sessões longas ou aceitar reconexão automática do Claude.

6. **Compatibilidade de clientes MCP** — nem todo cliente MCP suporta `url:` em vez de `command:`. Verificar Claude Desktop, Claude Code, Cursor, Cline antes de anunciar.

---

## Roadmap sugerido

### Fase 1 — Prova de conceito (MVP, Caminho A)
- Criar repo `expertintegrado/pipedrive-mcp-remote`
- Portar `index.js` para Workers (substituir stdio por HTTP, substituir `fs` por KV)
- Token por header
- Deploy em `pipedrive-mcp.expertintegrado.workers.dev`
- Testar com 2-3 mentorados internos

### Fase 2 — UX melhorada (Caminho B)
- Página de setup HTML (no mesmo Worker)
- Validação do token na criação da sessão
- URL personalizada por sessão
- Gerenciamento de tempo de vida e revogação
- Domínio customizado (`mcp.expertintegrado.com.br`)

### Fase 3 — OAuth oficial (Caminho C)
- Registrar app no Pipedrive Marketplace
- Fluxo OAuth 2.0 completo
- Painel de gerenciamento de contas conectadas
- Publicação no Marketplace do Pipedrive (opcional, para descoberta pública)

### Fase 4 — Hardening
- Rate limiting por sessão
- Observabilidade (métricas, logs estruturados, alertas)
- Backup de KV (export automático diário)
- Testes de carga

---

## O que a Expert precisa decidir

1. **Qual caminho começar?** Recomendação: A (MVP). Valida demanda antes de investir em OAuth.
2. **Domínio customizado?** Usar `mcp.expertintegrado.com.br` ou ficar em `*.workers.dev`?
3. **Quem administra a conta Cloudflare?** Criar conta corporativa ou usar pessoal de alguém?
4. **Versão local continua?** Sim — mentorados avançados/offline continuam podendo usar `npx`. Remoto é adicional, não substituto.
5. **Quando começar?** Definir prioridade na lista de skills a liberar (Pipedrive primeiro? Depois ClickUp, Zoom etc. seguem o mesmo padrão).

---

## Próximos passos (quando for executar)

1. Confirmar caminho (A/B/C) e plataforma (Cloudflare)
2. Criar conta Cloudflare corporativa da Expert
3. `wrangler login` + criar namespace KV + criar projeto Worker
4. Criar repo `expertintegrado/pipedrive-mcp-remote`
5. Portar código seguindo o plano da Fase 1
6. Testes internos, ajustes
7. Documentar no README do `pipedrive-mcp` principal apontando para a opção remota

---

## Referências

- [Anthropic — Remote MCP servers](https://docs.anthropic.com/en/docs/agents-and-tools/mcp)
- [Cloudflare — Build MCP servers on Workers](https://developers.cloudflare.com/agents/guides/remote-mcp-server/)
- [Pipedrive — Marketplace manager (OAuth apps)](https://pipedrive.readme.io/docs/marketplace-creating-a-proper-app)
- [Modelcontextprotocol — Transports spec](https://spec.modelcontextprotocol.io/specification/basic/transports/)
