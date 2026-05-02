# Configuração de Branch Protection — Padrão IANorth

> Checklist obrigatório após criar qualquer repositório na org `IANorth-Tecnologia`.
> Tempo estimado: 5 minutos por repositório.

---

## Por que isso existe

Sem branch protection, qualquer dev com acesso de Write pode:

- Dar push direto na `main` sem revisão
- Sobrescrever histórico com `git push --force`
- Deletar a `main` por engano
- Mergear código sem CI passar

Com branch protection configurada corretamente, **nada disso é possível** — nem para administradores. É a barreira que torna o processo de PR efetivamente obrigatório.

---

## ✅ Configuração padrão da branch `main`

Acesse o repositório no GitHub web → **Settings → Branches → Add branch protection rule**.

### Branch name pattern

```
main
```

### Regras a marcar

- [x] **Require a pull request before merging**
  - [x] Require approvals → **`1`**
  - [x] Dismiss stale pull request approvals when new commits are pushed
  - [x] Require review from Code Owners *(se houver arquivo `CODEOWNERS`)*
  - [ ] Require approval of the most recent reviewable push *(opcional, deixar desmarcado)*

- [x] **Require status checks to pass before merging**
  - [x] Require branches to be up to date before merging
  - Adicionar checks do CI configurado (ex.: `build`, `test`, `lint`)

- [x] **Require conversation resolution before merging**
  - Bloqueia merge se houver comentário de PR não resolvido

- [x] **Require linear history**
  - Força rebase ou squash, evita "merge commits" poluindo o histórico

- [x] **Require deployments to succeed before merging** *(apenas em repositórios com CD)*

- [x] **Lock branch** → **❌ NÃO marcar** *(isso bloqueia até PRs)*

- [x] **Do not allow bypassing the above settings**
  - **Crítico.** Sem isso, admins (você incluso) podem pular as regras. Marque sempre.

- [x] **Restrict who can push to matching branches**
  - Adicionar apenas: **Direção Técnica** (você)
  - Mesmo assim, na prática ninguém vai dar push direto — só merges via PR

### Regras a NÃO marcar

- [ ] **Allow force pushes** → ❌ desmarcado
- [ ] **Allow deletions** → ❌ desmarcado

Clicar em **Create** ou **Save changes**.

---

## ✅ Configuração da branch `develop` (se usar Gitflow)

Se o repositório usa Gitflow com branch `develop`, replicar a mesma configuração:

- Branch name pattern: `develop`
- Mesmas regras da `main`
- Approvals pode ser **`1`** também (ou flexibilizar para `0` se for ambiente de integração rápida — depende do projeto)

---

## ✅ Configuração de branches `release/*` e `hotfix/*`

Para repositórios com fluxo de release formal:

- Branch name pattern: `release/*` e `hotfix/*` (uma regra para cada)
- Mesmas regras da `main`
- **Restrict who can push:** apenas Direção Técnica

---

## 🎯 Como validar que ficou correto

Teste rápido após configurar. Em um repositório de teste:

```bash
git checkout main
echo "teste" >> README.md
git add README.md
git commit -m "test: tentando push direto"
git push origin main
```

**Resultado esperado:**

```
remote: error: GH006: Protected branch update failed for refs/heads/main.
remote: error: Changes must be made through a pull request.
```

Se aparecer essa mensagem, ✅ funcionou.

Se o push passou, alguma config não foi salva corretamente — refazer.

---

## 📁 Arquivo CODEOWNERS (recomendado)

Para forçar revisão automática de partes críticas do código, crie o arquivo `.github/CODEOWNERS` na raiz do repositório:

```
# Cada linha define: padrão de arquivo  →  responsável obrigatório

# Tudo por padrão precisa do review da Direção Técnica
*       @marlon

# Arquivos de infraestrutura e CI/CD
.github/        @marlon
Dockerfile      @marlon
docker-compose* @marlon

# Arquivos sensíveis de configuração
*.env.example   @marlon
config/         @marlon

# Documentação técnica pode ter outros revisores
docs/           @marlon
*.md            @marlon
```

Substitua `@marlon` pelo seu username GitHub real. Adicione outros revisores quando o time crescer.

> 💡 Quando alguém abrir PR mexendo em arquivo coberto pelo CODEOWNERS, o GitHub solicita review automaticamente do dono.

---

## 🚦 Aplicar em todos os repositórios existentes

Repositórios já criados precisam ser configurados manualmente. Checklist:

- [ ] `IANorth-Tecnologia/sinobras-app1`
- [ ] `IANorth-Tecnologia/sinobras-app2`
- [ ] `IANorth-Tecnologia/sinobras-app3`
- [ ] `IANorth-Tecnologia/sinobras-app4`
- [ ] *(adicionar outros conforme existam)*

Para cada um:

1. Aplicar configuração padrão de `main`
2. Adicionar arquivo `CODEOWNERS` via PR
3. Validar com o teste de push direto

---

## 🤖 Automação (opcional, para o futuro)

Quando o time crescer, vale automatizar isso via GitHub API. Existem duas opções:

### Opção 1 — Repository Rulesets (recomendado pelo GitHub atualmente)

A nível de organização: **Org Settings → Repository → Rulesets → New ruleset**.

Cria uma regra que se aplica automaticamente a todos os repositórios novos da org. Define em um único lugar e propaga.

### Opção 2 — Script via GitHub CLI

```bash
gh api -X PUT \
  repos/IANorth-Tecnologia/<repo>/branches/main/protection \
  --input branch-protection.json
```

Onde `branch-protection.json` é um arquivo JSON com a configuração padrão. Útil para aplicar em massa.

> 📌 **Recomendação:** quando vocês passarem de 5 repositórios ativos, configurem **Repository Rulesets** a nível de org. Manutenção fica muito mais simples.

---

## 🚫 Erros comuns

| Erro | Causa | Solução |
|---|---|---|
| Admin consegue dar push direto | Esqueceu de marcar "Do not allow bypassing" | Editar regra e marcar |
| CI required check não aparece | Pipeline ainda não rodou nem uma vez | Disparar um build, aguardar terminar, voltar e selecionar |
| Branch sem proteção em repos antigos | Configuração não retroativa | Aplicar manualmente conforme checklist acima |
| CODEOWNERS não dispara review | Sintaxe errada ou usuário sem acesso ao repo | Validar sintaxe na aba Settings → Code owners |

---

## 📋 Convenção: quem aprova o quê

Enquanto a Direção Técnica é o único reviewer obrigatório:

| Tipo de PR | Aprovador obrigatório |
|---|---|
| Qualquer PR em `main` | Direção Técnica |
| Mudança em `Dockerfile`, CI/CD, config | Direção Técnica |
| Mudança em arquivos com CODEOWNER | Owner definido |
| PR em ambiente de dev/sandbox | Pode ser flexibilizado conforme projeto |

Quando o time crescer, adicione tech leads como aprovadores adicionais — sempre mantendo **mínimo 1 aprovação obrigatória**.

---

> Dúvidas? Canal `🙋 duvidas-juniores` na Equipe Engenharia.
