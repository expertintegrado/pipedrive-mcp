# Release

Publicação no npm é automática via GitHub Actions quando uma tag `v*` é pushada. **Não rode `npm publish` manualmente** — a máquina local não tem (nem deve ter) credenciais de publicação.

Este documento é executável por uma IA. Se você é humano, peça ao Claude Code: *"Faça uma release patch/minor/major seguindo o RELEASING.md."*

---

## Pré-requisitos (verificar antes de começar)

```bash
# 1. Deve estar na main atualizada
git rev-parse --abbrev-ref HEAD   # deve retornar: main
git fetch && git status -uno      # deve dizer: up to date with origin/main

# 2. Working tree limpa
git status --porcelain            # deve retornar vazio

# 3. Secret NPM_TOKEN existe no GitHub
gh secret list --repo expertintegrado/pipedrive-mcp | grep -q NPM_TOKEN || echo "FALTA NPM_TOKEN"
```

Se qualquer verificação falhar, pare e corrija antes de prosseguir.

---

## Escolha do tipo de bump

Seguimos [SemVer](https://semver.org/lang/pt-BR/):

| Tipo | Usar quando | Exemplo |
|---|---|---|
| `patch` | Bug fix, ajuste de docs, dependência interna (sem mudança de comportamento visível) | 6.1.1 → 6.1.2 |
| `minor` | Feature nova compatível, nova tool, mudança de UX que não quebra configs existentes | 6.1.2 → 6.2.0 |
| `major` | Breaking change (muda configuração, env var, ou comportamento público existente) | 6.2.0 → 7.0.0 |

Regra de ouro: **se algum usuário já instalado precisa mudar algo pra continuar funcionando, é major**.

---

## Procedimento

Execute na ordem, sem pular etapas. Substitua `<TIPO>` por `patch`, `minor` ou `major`.

### 1. Atualizar CHANGELOG.md

Calcule a próxima versão e a data de hoje:

```bash
CURRENT=$(node -p "require('./package.json').version")
NEXT=$(node -p "const [M,m,p]=require('./package.json').version.split('.').map(Number); const t='<TIPO>'; t==='major'?\`\${M+1}.0.0\`:t==='minor'?\`\${M}.\${m+1}.0\`:\`\${M}.\${m}.\${p+1}\`")
TODAY=$(date +%Y-%m-%d)
echo "Bumping $CURRENT -> $NEXT ($TODAY)"
```

Adicione uma seção no topo do `CHANGELOG.md` (abaixo do cabeçalho, acima da versão atual) no formato:

```markdown
## [$NEXT] — $TODAY

### Adicionado
- <itens novos, se houver>

### Mudou
- <mudancas em comportamento existente>

### Corrigido
- <bugs corrigidos>
```

Omita as seções vazias. Liste cada item com o **porquê** da mudança, não só o "quê".

### 2. Commitar o CHANGELOG

```bash
git add CHANGELOG.md
git commit -m "docs(changelog): entrada para $NEXT"
```

### 3. Bump version + criar tag

O comando `npm version` atualiza `package.json`, cria um commit e cria a tag num passo só:

```bash
npm version <TIPO>
# Saída esperada: "v$NEXT"
```

Verificação:

```bash
# Versão do package.json bate com a tag
test "$(node -p 'require("./package.json").version')" = "$(git describe --tags --exact-match)" \
  && echo "OK: package.json e tag sincronizados" \
  || echo "ERRO: desincronizado"
```

### 4. Push

```bash
git push origin main
git push origin "v$NEXT"
```

### 5. Acompanhar o workflow

```bash
sleep 5
gh run watch --exit-status
```

Esperado: job `publish` completa em ~15s, verde.

### 6. Verificar publicação

```bash
# Versão no registry deve ser a nova
npm view @expertintegrado/pipedrive-mcp version

# GitHub Release deve existir
gh release view "v$NEXT" --repo expertintegrado/pipedrive-mcp
```

Ambos devem mostrar `$NEXT`.

---

## Falhas comuns

### Workflow falha: "Unauthorized" / "403" no `npm publish`

O `NPM_TOKEN` expirou ou foi revogado.

```bash
# Confirmar que o secret existe:
gh secret list --repo expertintegrado/pipedrive-mcp | grep NPM_TOKEN
```

Ação: gerar novo Granular Access Token em https://www.npmjs.com/settings/~/tokens com:
- Bypass 2FA ✅
- Read and write
- Scope no package `@expertintegrado/pipedrive-mcp`

Atualizar secret:
```bash
gh secret set NPM_TOKEN --repo expertintegrado/pipedrive-mcp
```

Refazer apenas o último passo (re-push da tag não funciona — já foi consumida). Use `workflow_dispatch`:

```bash
gh workflow run release.yml --repo expertintegrado/pipedrive-mcp
```

### Workflow falha: "version already published"

A versão do `package.json` já existe no npm. Não é possível republicar a mesma versão.

Ação: apague a tag local e remota, bumpe de novo:

```bash
git tag -d "v$NEXT"
git push origin ":v$NEXT"
# Volte ao passo 3 com um bump maior (ex: se tentou patch, vá pra minor)
```

### Esqueci de atualizar o CHANGELOG antes do `npm version`

A tag já foi criada apontando pra um commit sem CHANGELOG. Não precisa refazer:

```bash
# Adicionar entrada no CHANGELOG.md como descrito no passo 1
git add CHANGELOG.md
git commit -m "docs(changelog): entrada para $NEXT (post-bump)"
git push origin main
```

A release já publicada no npm não mudará, mas a `main` fica consistente. Na próxima release, o CHANGELOG estará correto.

### Preciso reverter uma publicação

**Não é possível sobrescrever uma versão no npm.** Opções:

1. **Preferencial:** publicar uma versão nova (`patch`) com a correção.
2. **Excepcional:** `npm unpublish @expertintegrado/pipedrive-mcp@X.Y.Z` dentro de 72h da publicação. Desencorajado — quebra instalações de quem já pegou a versão.

---

## Checklist rápido

```
[ ] Pré-requisitos passam (main atualizada, tree limpa, NPM_TOKEN existe)
[ ] CHANGELOG.md tem entrada nova com data de hoje
[ ] git commit do CHANGELOG
[ ] npm version <patch|minor|major>
[ ] git push origin main
[ ] git push origin v<versão>
[ ] gh run watch --exit-status passa verde
[ ] npm view @expertintegrado/pipedrive-mcp version retorna a nova versão
```
