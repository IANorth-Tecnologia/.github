# Como abrir um Pull Request na IANorth

> Guia rápido para devs. Vale para todos os repositórios da org `IANorth-Tecnologia`.

---

## Antes de começar

- [ ] Repositório clonado localmente
- [ ] Acesso de **Write** ao repositório
- [ ] Branch `main` atualizada (`git pull origin main`)

---

## 🌿 Passo 1 — Criar sua branch

**Nunca commite direto na `main`.** Sempre crie uma branch própria.

> 🛡️ **Sobre a proteção da `main`:** a branch `main` é **protegida no GitHub** — ninguém consegue fazer push direto, force-push, deletar, ou mergear sem aprovação da Direção Técnica. Mas isso **não impede** você de rodar `git checkout main` ou `git pull origin main` localmente — esses comandos são leitura, totalmente seguros e necessários para manter sua branch atualizada. A proteção atua sobre **escritas no remoto**, não sobre operações locais.

### Convenção de nome

```
<tipo>/<descrição-curta-em-kebab-case>
```

| Tipo | Quando usar | Exemplo |
|---|---|---|
| `feat` | Nova funcionalidade | `feat/login-com-2fa` |
| `fix` | Correção de bug | `fix/timeout-endpoint-predict` |
| `refactor` | Refatoração sem mudança de comportamento | `refactor/extrair-servico-auth` |
| `docs` | Apenas documentação | `docs/atualizar-readme-deploy` |
| `chore` | Tarefas de build, dependências, configs | `chore/atualizar-dependencias` |
| `test` | Adição ou ajuste de testes | `test/cobertura-modulo-billing` |

### Comandos

```bash
git checkout main
git pull origin main
git checkout -b feat/login-com-2fa
```

---

## 💻 Passo 2 — Trabalhar e commitar

### Conventional Commits (obrigatório)

```
<tipo>: <mensagem curta no imperativo>
```

Exemplos:

```bash
git commit -m "feat: adiciona endpoint de login com 2FA"
git commit -m "fix: corrige timeout no endpoint /predict"
git commit -m "refactor: extrai lógica de auth para serviço dedicado"
git commit -m "docs: atualiza README com instruções de deploy"
```

✅ **Bons commits:**
- Mensagem no imperativo ("adiciona", não "adicionado")
- Curta (até 72 caracteres na primeira linha)
- Específica ao que mudou

❌ **Commits ruins:**
- `git commit -m "wip"`
- `git commit -m "ajustes"`
- `git commit -m "fix bug"` (qual bug?)

### Antes de subir, revise você mesmo

```bash
git status              # confere o que vai subir
git diff --staged       # revisa as mudanças
```

---

## 🚀 Passo 3 — Subir a branch

```bash
git push -u origin feat/login-com-2fa
```

O `-u` configura o tracking — nos próximos pushes basta `git push`.

---

## 📝 Passo 4 — Abrir o Pull Request

### Pelo VS Code

1. Instale a extensão **"GitHub Pull Requests"** (uma vez só)
2. Aba **GitHub** no menu lateral → **Create Pull Request**
3. Confirme: base `main` ← compare `sua-branch`
4. Preencher título e descrição (modelo abaixo)
5. **Create**

### Pelo PyCharm

1. Menu **Git → GitHub → Create Pull Request**
2. Confirme: base `main` ← compare `sua-branch`
3. Preencher título e descrição
4. **Create Pull Request**

### Pelo navegador

1. Acessar `github.com/IANorth-Tecnologia/<repo>`
2. Aparece um banner amarelo "Compare & pull request" → clicar
3. Preencher e **Create pull request**

---

## 📋 Modelo de descrição de PR

Cole isso na descrição e preencha. Salva tempo de todo mundo no review.

```markdown
## O que muda
Descreva em 2-3 linhas o que este PR faz.

## Por quê
Contexto do problema ou da feature. Link para issue/tarefa do Planner se houver.

## Como testar
1. Passo a passo para o revisor validar.
2. Comandos, endpoints, dados de exemplo.
3. Resultado esperado.

## Checklist
- [ ] Testei localmente
- [ ] Adicionei/atualizei testes automatizados
- [ ] Atualizei documentação relevante
- [ ] Não introduzi secrets ou credenciais no código
- [ ] CI está verde

## Screenshots (se aplicável)
<colocar imagens aqui>
```

---

## 👀 Passo 5 — Solicitar review

- **Reviewers:** marque a Direção Técnica (obrigatório) + um par técnico se fizer sentido
- **Assignees:** você mesmo
- **Labels:** marque com `feat`, `bug`, `urgent`, etc.
- **Avise no Teams:** mande no canal `🔍 code-review` da Equipe Engenharia mencionando o PR

> 💡 O bot do GitHub no Teams pode notificar automaticamente — mas mencionar pessoalmente acelera o review quando é prioritário.

---

## 🔄 Passo 6 — Responder ao review

### Se houver comentários pedindo mudanças

1. Faça os ajustes localmente
2. Commit e push na **mesma branch** (não abra PR novo)
   ```bash
   git add .
   git commit -m "fix: ajusta validação conforme review"
   git push
   ```
3. Marque cada comentário resolvido como **Resolve conversation**
4. Re-solicite review clicando no ícone 🔄 ao lado do nome do reviewer

### Se discordar de um comentário

- **Discuta na thread do PR**, com argumento técnico
- Não ignore — comentário sem resposta trava o merge
- Se o impasse persistir, escale para Direção Técnica decidir

### Se o review aprovou

- Resolva qualquer comentário pendente não-bloqueante
- Aguarde merge pela Direção Técnica (juniores não fazem merge direto na `main`)

---

## 🧹 Passo 7 — Após o merge

```bash
git checkout main
git pull origin main
git branch -d feat/login-com-2fa
```

A branch remota é deletada automaticamente após merge (configuração da org).

---

## 🚫 Erros comuns

| Erro | Causa | Solução |
|---|---|---|
| `Your branch is behind 'origin/main'` | `main` avançou enquanto você trabalhava | `git pull origin main --rebase` na sua branch |
| Conflito de merge | Mesma linha alterada por outro PR | Resolver localmente, commitar, dar push |
| `Required status check is failing` | CI quebrou | Olhar logs do GitHub Actions, corrigir e dar push |
| `Review required from Code Owners` | PR mexeu em arquivo crítico | Aguardar review da pessoa marcada como CODEOWNER |
| Acidentalmente commitei na `main` local | Não criou branch antes | `git reset --soft HEAD~1` → criar branch → commitar lá |

---

## 📏 Boas práticas

### Tamanho do PR

- **Ideal:** menos de 400 linhas alteradas
- **Aceitável:** até 800 linhas
- **Exige justificativa:** acima de 800 linhas

PR pequeno = review rápido = merge rápido. Quebre features grandes em PRs menores quando possível.

### Frequência de commits

- Commits pequenos e frequentes durante o desenvolvimento
- Não precisa fazer "squash" antes do PR — a Direção Técnica decide o método de merge

### Code review etiquette

**Como reviewer:**
- Critique código, não a pessoa
- Sugira alternativas, não apenas aponte problemas
- Marque diferença entre **bloqueante** ("isso precisa mudar") e **sugestão** ("considera isso")
- Aprove com confiança quando o código está bom — não fique nitpicando vírgulas

**Como autor:**
- Não leve críticas para o pessoal
- Responda todos os comentários, mesmo que seja "ok, ajustado" ou "vou deixar como está porque X"
- Agradeça reviews boas — cultura saudável de time

---

## ⚡ Fluxo resumido

```
1. git checkout main && git pull
2. git checkout -b feat/minha-feature
3. <código> + commits seguindo Conventional Commits
4. git push -u origin feat/minha-feature
5. Abrir PR com descrição preenchida
6. Solicitar review + avisar no #code-review
7. Responder comentários até aprovação
8. Aguardar merge
9. Limpar branch local
```

---

> Dúvidas? Canal `🙋 duvidas-juniores` na Equipe Engenharia.
