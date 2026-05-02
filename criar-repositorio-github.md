# Como criar um repositório na organização IANorth-Tecnologia

> Guia rápido para devs. Use VS Code **ou** PyCharm — escolha sua ferramenta preferida.

---

## Antes de começar (uma vez só)

- [ ] Ter conta GitHub ativa e acesso à org `IANorth-Tecnologia`
- [ ] Ter Git instalado (`git --version` no terminal pra confirmar)
- [ ] Estar autenticado no IDE escolhido (passos abaixo)

---

## 🟦 Opção A — VS Code

### A.1 Autenticar no GitHub (uma vez só)

1. Abrir VS Code
2. Clicar no ícone de **conta** (canto inferior esquerdo) → **Sign in with GitHub**
3. Confirmar autorização no navegador

### A.2 Criar o repositório

1. Abrir a pasta do projeto: **File → Open Folder**
2. Abrir o terminal integrado: **Ctrl + `** (ou **View → Terminal**)
3. Inicializar Git:
   ```bash
   git init
   git add .
   git commit -m "chore: initial commit"
   ```
4. Abrir a paleta de comandos: **Ctrl + Shift + P**
5. Digitar e selecionar: **`Git: Publish to GitHub`**
6. Escolher:
   - Tipo: **`Publish to GitHub private repository`** (sempre privado)
   - Org: **`IANorth-Tecnologia`** (não publique na sua conta pessoal)
   - Nome: `nome-do-projeto` (minúsculas, hífens, sem acentos)
7. Pronto — o repo aparece em `github.com/IANorth-Tecnologia/nome-do-projeto`

---

## 🟧 Opção B — PyCharm

### B.1 Autenticar no GitHub (uma vez só)

1. Abrir PyCharm
2. **File → Settings → Version Control → GitHub**
3. Clicar em **+** → **Log in via GitHub** → autorizar no navegador

### B.2 Criar o repositório

1. Abrir o projeto: **File → Open**
2. Menu **Git → GitHub → Share Project on GitHub**
3. Preencher:
   - **Repository name**: `nome-do-projeto`
   - **Organization**: selecionar **`IANorth-Tecnologia`**
   - Marcar ✅ **Private**
   - **Description**: descrição curta do projeto
4. Clicar em **Share**
5. Na janela seguinte, marcar todos os arquivos relevantes e clicar em **Add**
6. Pronto — o repo aparece em `github.com/IANorth-Tecnologia/nome-do-projeto`

---

## ✅ Configurações obrigatórias após criar

Faça essas configurações **antes** de adicionar a equipe ao repositório.

### 1. Adicionar `.gitignore` e `README.md`

Se não criou junto com o projeto, adicione manualmente:

```bash
# .gitignore — exemplo Python básico
__pycache__/
*.py[cod]
.env
.venv/
venv/
.idea/
.vscode/
*.log
```

### 2. Configurar Branch Protection

No GitHub web, abrir o repositório → **Settings → Branches → Add rule**:

- Branch: `main`
- ✅ Require a pull request before merging
- ✅ Require approvals: **1**
- ✅ Dismiss stale pull request approvals when new commits are pushed
- ✅ Require status checks to pass before merging
- ✅ Do not allow bypassing the above settings

### 3. Adicionar a equipe

Em **Settings → Collaborators and teams → Add teams**:

- Adicionar time apropriado (ex.: `core-devs`, `juniores`)
- Permissão: **Write** (jamais Admin para juniores)

### 4. Conectar ao Teams

No canal `📦 commits-prs` da Equipe do projeto:

```
@github subscribe IANorth-Tecnologia/nome-do-projeto commits pulls issues
```

---

## 🚫 Erros comuns

| Erro | Causa | Solução |
|---|---|---|
| Repositório criado na conta pessoal | Esqueceu de selecionar a org | Excluir e refazer escolhendo `IANorth-Tecnologia` |
| Push rejeitado por branch protection | Tentou commit direto na `main` | Criar branch própria, fazer PR |
| Org não aparece na lista | Falta autorização SSO | Acessar `github.com/orgs/IANorth-Tecnologia` e autorizar SSO no navegador |
| `.env` apareceu no commit | `.gitignore` não estava antes do `git add` | `git rm --cached .env` e refazer commit |

---

## 📋 Convenções obrigatórias

- **Nome do repositório:** minúsculas, hífens, sem acentos (`sinobras-monitor`, não `Sinobras_Monitor`)
- **Privacidade:** sempre **privado**, jamais público sem aprovação da Direção Técnica
- **Branch principal:** `main` (não `master`)
- **Commits:** seguir Conventional Commits (`feat:`, `fix:`, `chore:`, `docs:`)
- **Secrets:** nunca commitar — usar variáveis de ambiente ou Azure Key Vault

---

> Dúvidas? Canal `🙋 duvidas-juniores` na Equipe Engenharia.
