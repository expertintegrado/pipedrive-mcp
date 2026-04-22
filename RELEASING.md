# Procedimento de Release

Este repositório publica no npm automaticamente quando uma tag `v*` é enviada ao GitHub. **Não rode `npm publish` manualmente** — a máquina local não deve ter credenciais de publicação.

## Pré-requisitos

- Permissão **Write** no repositório
- Todas as mudanças desejadas já mergeadas na `main`
- `git` configurado com seu usuário

## Passo a passo

### 1. Puxe a `main` atualizada

```bash
git checkout main
git pull
```

### 2. Decida o tipo de bump

Seguimos [SemVer](https://semver.org/lang/pt-BR/):

| Tipo | Quando usar | Exemplo |
|---|---|---|
| `patch` | Correção de bug, ajuste de docs, dependência interna | `6.1.1 → 6.1.2` |
| `minor` | Funcionalidade nova compatível, nova tool, mudança de UX relevante | `6.1.2 → 6.2.0` |
| `major` | Breaking change (mudança que quebra configuração ou comportamento existente) | `6.2.0 → 7.0.0` |

**Na dúvida, é `minor`.** Nunca use `patch` pra feature nova.

### 3. Atualize o `CHANGELOG.md`

Abra `CHANGELOG.md` e adicione a nova entrada **no topo** (abaixo do cabeçalho), seguindo o formato existente:

```markdown
## [X.Y.Z] — YYYY-MM-DD

### Adicionado
- ...

### Mudou
- ...

### Corrigido
- ...
```

Use hoje como data.

### 4. Bump da versão + criação da tag

Use o comando `npm version` — ele bumpa `package.json`, faz um commit e cria a tag num só passo:

```bash
npm version patch   # 6.1.1 → 6.1.2
# ou
npm version minor   # 6.1.1 → 6.2.0
# ou
npm version major   # 6.1.1 → 7.0.0
```

Verifique:

```bash
git log -1               # último commit deve ser "v6.1.2" ou similar
git tag --list 'v*' --sort=-v:refname | head   # tag nova no topo
cat package.json | grep '"version"'            # bate com a tag
```

### 5. Commite o `CHANGELOG.md` junto

O `npm version` cria o commit só do `package.json`. Precisamos juntar o CHANGELOG:

```bash
git add CHANGELOG.md
git commit --amend --no-edit
# Recria a tag apontando pro commit amended:
git tag -f v$(node -p "require('./package.json').version")
```

> Fluxo alternativo: rode `npm version` **depois** de commitar o CHANGELOG separado na `main` (via PR). Aí não precisa amend.

### 6. Push do commit + tag

```bash
git push origin main
git push origin v$(node -p "require('./package.json').version")
```

### 7. Acompanhe a publicação

```bash
gh run watch --exit-status
```

Ou abra https://github.com/expertintegrado/pipedrive-mcp/actions — o workflow `Release` deve rodar em ~15 segundos e:

1. Publicar o pacote no npm (`@expertintegrado/pipedrive-mcp@X.Y.Z`)
2. Criar uma GitHub Release com release notes automáticas

### 8. Verifique

```bash
npm view @expertintegrado/pipedrive-mcp version
# Deve mostrar a versão que você acabou de publicar
```

E confira https://www.npmjs.com/package/@expertintegrado/pipedrive-mcp — a nova versão deve aparecer no topo.

---

## O que fazer se o workflow falhar

### Erro: versão do `package.json` não bate com a tag

O workflow valida isso. Se cair nesse erro, significa que você criou a tag apontando pra um commit onde `package.json` ainda estava na versão antiga. Delete a tag e refaça:

```bash
git tag -d v6.1.2
git push origin :v6.1.2      # apaga no remoto
# Corrija o package.json, commite, e recrie a tag
```

### Erro: 401/403 no npm publish

Token do secret `NPM_TOKEN` expirou ou foi revogado. Admin gera um novo em https://www.npmjs.com/settings/~/tokens (Granular, bypass 2FA, Read+Write no package) e atualiza o secret em https://github.com/expertintegrado/pipedrive-mcp/settings/secrets/actions.

### Publiquei a versão errada por engano

**Não dá pra sobrescrever uma versão no npm.** Publique uma versão nova (bump patch) com a correção. Em casos excepcionais, `npm unpublish` é possível **em até 72h** após a publicação, mas é desencorajado — prefira publicar uma versão nova.

---

## Checklist rápido

- [ ] `main` atualizada localmente
- [ ] Entrada no `CHANGELOG.md` com data de hoje
- [ ] `npm version <patch|minor|major>` rodado
- [ ] `CHANGELOG.md` amendado no commit de bump
- [ ] `git push origin main`
- [ ] `git push origin vX.Y.Z`
- [ ] Workflow passou verde
- [ ] `npm view @expertintegrado/pipedrive-mcp version` retorna a versão nova
