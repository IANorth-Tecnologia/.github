# Guia de Contribuição — IANorth Tecnologia

Bem-vindo! Este documento descreve como contribuímos com código nos projetos da IANorth.

---

## 🌳 Estratégia de branches

Adotamos um modelo simplificado baseado em **GitHub Flow** com adaptações para nossos ciclos de release:

```
main          ← código em produção (sempre estável)
  └── develop ← integração contínua (próxima release)
        ├── feature/<descrição>
        ├── fix/<descrição>
        └── chore/<descrição>
```

### Convenção de nomes de branch

- `feature/sensor-temperatura` — nova funcionalidade
- `fix/login-timeout` — correção de bug
- `chore/atualizar-deps` — manutenção
- `docs/api-iot` — documentação
- `hotfix/erro-producao-sinobras` — correção urgente em produção

---

## 📝 Conventional Commits

Mensagens de commit seguem o padrão [Conventional Commits](https://www.conventionalcommits.org/pt-br/):

```
<tipo>(<escopo opcional>): <descrição curta>

<corpo opcional explicando o que e por que>

<rodapé opcional: BREAKING CHANGE, refs, closes>
```

### Tipos aceitos

| Tipo       | Quando usar                                       |
|------------|---------------------------------------------------|
| `feat`     | Nova funcionalidade                               |
| `fix`      | Correção de bug                                   |
| `docs`     | Apenas documentação                               |
| `style`    | Formatação, sem mudança de código                 |
| `refactor` | Refatoração sem mudança funcional                 |
| `perf`     | Melhoria de performance                           |
| `test`     | Adicionar ou corrigir testes                      |
| `build`    | Mudanças em build, dependências                   |
| `ci`       | Mudanças em CI/CD                                 |
| `chore`    | Tarefas de manutenção                             |
| `revert`   | Reverter commit anterior                          |

### Exemplos

```
feat(iot): adicionar leitura de sensor de lidar
fix(auth): corrigir timeout de sessão após 15min
docs: atualizar README com instruções de deploy
chore(deps): atualizar fastapi para 0.115
```

Para breaking changes, adicione `!` ou rodapé:
```
feat(api)!: alterar formato de resposta do endpoint /sensors

BREAKING CHANGE: o campo "value" agora é objeto, não número.
```

---

## 🔄 Fluxo de trabalho

1. **Crie uma issue** descrevendo o trabalho (use os templates).
2. **Crie uma branch** a partir de `develop`:
   ```bash
   git checkout develop
   git pull origin develop
   git checkout -b feature/minha-feature
   ```
3. **Commits pequenos e focados**, seguindo Conventional Commits.
4. **Abra um Pull Request** para `develop` (preencha o template).
5. **Aguarde review** — pelo menos 1 aprovação obrigatória.
6. **CI deve passar** (lint, testes, build).
7. **Merge via Squash** (mantém histórico limpo na develop).

### Quando vai pra produção

- `develop` → `main` apenas via PR de release.
- Tagging automático segue [SemVer](https://semver.org/lang/pt-BR/): `v1.2.3`.

---

## ✅ Checklist antes de abrir PR

- [ ] Código testado localmente
- [ ] Testes automatizados adicionados/atualizados
- [ ] Lint passando sem warnings
- [ ] Sem `console.log`, `print()` ou código de debug
- [ ] Sem credenciais, tokens ou secrets no código
- [ ] Documentação atualizada se necessário
- [ ] Se afeta produção (Sinobras), avisei no PR

---

## 🔍 Code Review

### Como revisor, você deve verificar:
- O código resolve o problema descrito?
- Há testes adequados?
- Há riscos para sistemas em produção?
- O código é legível e mantível?
- Há vulnerabilidades de segurança?
- Performance é aceitável?

### Como autor, você deve:
- Responder a todos os comentários
- Não tomar críticas como pessoais — é sobre o código
- Solicitar nova review após resolver os pontos

---

## 🚨 Hotfixes em produção

Para correções urgentes em produção (ex.: incidente no Sinobras):

```bash
git checkout main
git checkout -b hotfix/<descrição>
# ... corrige ...
# PR direto para main, depois cherry-pick para develop
```

Hotfixes podem ter review reduzido, mas **nunca** dispensam review.

---

## 📞 Dúvidas?

A comunicação oficial da engenharia acontece no Microsoft Teams. Envie sua pergunta no canal `💻 dev` do seu projeto ou, se for júnior, sinta-se à vontade para perguntar no canal `🙋 duvidas` da Equipe de Engenharia."


