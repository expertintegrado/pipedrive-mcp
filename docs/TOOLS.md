# Ferramentas disponíveis

24 tools expostas pelo MCP, agrupadas por domínio.

## Sincronização

| Ferramenta | Descrição |
|---|---|
| `sync_all` | Sincroniza TUDO em um único `config.js` (campos, atividades, pipelines, usuários, domínio) |

## Negócios (deals)

| Ferramenta | Descrição |
|---|---|
| `list_deals` | Lista com filtros por status, pipeline, etapa, responsável |
| `search_deals` | Busca por termo |
| `get_deal` | Detalhes completos com campos legíveis |
| `create_deal` | Cria negócio com campos personalizados (guardrail anti-duplicata) |
| `update_deal` | Atualiza status, etapa, valor, responsável |
| `get_deal_flow` | Histórico de movimentações de status e etapa |
| `update_deal_fields` | Atualiza campos personalizados com proteção contra sobrescrita |

## Contatos (persons)

| Ferramenta | Descrição |
|---|---|
| `search_persons` | Busca por nome, email ou telefone |
| `get_person` | Detalhes completos |
| `create_person` | Cria contato (guardrail anti-duplicata por telefone/email) |
| `update_person` | Atualiza nome, email, telefone, organização (com proteção) |

## Organizações

| Ferramenta | Descrição |
|---|---|
| `search_organizations` | Busca por nome |
| `get_organization` | Detalhes completos |
| `create_organization` | Cria organização (guardrail anti-duplicata por nome) |

## Atividades

| Ferramenta | Descrição |
|---|---|
| `list_activities` | Lista com filtros por tipo, usuário, período |
| `list_deal_activities` | Lista todas as atividades de um negócio |
| `create_activity` | Cria atividade com alias e duração configurável |
| `update_activity` | Atualiza atividade (remarcar, concluir, mudar tipo, vincular deal) |

## Notas

| Ferramenta | Descrição |
|---|---|
| `create_note` | Cria nota em negócio, contato ou organização |
| `update_note` | Edita nota existente (pinar/despinar) |
| `list_deal_notes` | Lista notas de um negócio |

## Produtos

| Ferramenta | Descrição |
|---|---|
| `list_products` | Lista produtos disponíveis |
| `add_product_to_deal` | Vincula produto a negócio com preço e quantidade |

---

## Guardrails

Algumas operações têm verificações automáticas que podem ser contornadas com `force: true` após confirmação explícita do usuário.

| Operação | Verificação automática |
|----------|----------------------|
| `create_person` | Busca por últimos 8 dígitos do telefone + email |
| `create_deal` | Busca deals abertos para o `person_id` |
| `create_organization` | Busca organizações por nome |
| `create_activity` | Busca atividades pendentes do deal/pessoa |
| `update_person` | Verifica conflitos antes de sobrescrever nome/org |
| `update_deal_fields` | Verifica conflitos em campos customizados |

## Resolução por nome

Campos como pipeline, etapa e usuário podem ser passados pelo **nome**, **ID** ou **alias** — o MCP resolve automaticamente via `config.js` carregado no startup.
