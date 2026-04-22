# Ferramentas disponiveis

24 tools expostas pelo MCP, agrupadas por dominio.

## Sincronizacao

| Ferramenta | Descricao |
|---|---|
| `sync_all` | Sincroniza TUDO em um unico `config.js` (campos, atividades, pipelines, usuarios, dominio) |

## Negocios (deals)

| Ferramenta | Descricao |
|---|---|
| `list_deals` | Lista com filtros por status, pipeline, etapa, responsavel |
| `search_deals` | Busca por termo |
| `get_deal` | Detalhes completos com campos legiveis |
| `create_deal` | Cria negocio com campos personalizados (guardrail anti-duplicata) |
| `update_deal` | Atualiza status, etapa, valor, responsavel |
| `get_deal_flow` | Historico de movimentacoes de status e etapa |
| `update_deal_fields` | Atualiza campos personalizados com protecao contra sobrescrita |

## Contatos (persons)

| Ferramenta | Descricao |
|---|---|
| `search_persons` | Busca por nome, email ou telefone |
| `get_person` | Detalhes completos |
| `create_person` | Cria contato (guardrail anti-duplicata por telefone/email) |
| `update_person` | Atualiza nome, email, telefone, organizacao (com protecao) |

## Organizacoes

| Ferramenta | Descricao |
|---|---|
| `search_organizations` | Busca por nome |
| `get_organization` | Detalhes completos |
| `create_organization` | Cria organizacao (guardrail anti-duplicata por nome) |

## Atividades

| Ferramenta | Descricao |
|---|---|
| `list_activities` | Lista com filtros por tipo, usuario, periodo |
| `list_deal_activities` | Lista todas as atividades de um negocio |
| `create_activity` | Cria atividade com alias e duracao configuravel |
| `update_activity` | Atualiza atividade (remarcar, concluir, mudar tipo, vincular deal) |

## Notas

| Ferramenta | Descricao |
|---|---|
| `create_note` | Cria nota em negocio, contato ou organizacao |
| `update_note` | Edita nota existente (pinar/despinar) |
| `list_deal_notes` | Lista notas de um negocio |

## Produtos

| Ferramenta | Descricao |
|---|---|
| `list_products` | Lista produtos disponiveis |
| `add_product_to_deal` | Vincula produto a negocio com preco e quantidade |

---

## Guardrails

Algumas operacoes tem verificacoes automaticas que podem ser contornadas com `force: true` apos confirmacao explicita do usuario.

| Operacao | Verificacao automatica |
|----------|----------------------|
| `create_person` | Busca por ultimos 8 digitos do telefone + email |
| `create_deal` | Busca deals abertos para o `person_id` |
| `create_organization` | Busca organizacoes por nome |
| `create_activity` | Busca atividades pendentes do deal/pessoa |
| `update_person` | Verifica conflitos antes de sobrescrever nome/org |
| `update_deal_fields` | Verifica conflitos em campos customizados |

## Resolucao por nome

Campos como pipeline, etapa e usuario podem ser passados pelo **nome**, **ID** ou **alias** — o MCP resolve automaticamente via `config.js` carregado no startup.
