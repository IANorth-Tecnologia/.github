# Guia de Configuração do Microsoft Teams — IANorth

> Documento operacional. Anexo ao **Playbook de Onboarding de Engenharia** e às **Convenções de Uso do Teams**.
> Use este guia como checklist sequencial para implementar o ambiente colaborativo do zero.

| | |
|---|---|
| **Versão** | 1.0 |
| **Última atualização** | Abril/2026 |
| **Tempo estimado de implementação** | 4 a 6 horas (primeira execução completa) |
| **Pré-requisitos** | Conta Microsoft 365 com perfil de **Administrador Global** ou **Administrador do Teams** |

---

## Como usar este guia

Cada seção é independente e idempotente — você pode parar e retomar sem problema. Marque os checkboxes à medida que avança. Recomendo executar na ordem apresentada na primeira vez; nas próximas Equipes, você pode usar a Etapa 9 (modelagem) para acelerar.

> ⚠️ **Aviso importante.** A Microsoft descontinuou os *Incoming Webhooks* tradicionais em **30 de abril de 2026**. Este guia usa o método atual via **Workflows (Power Automate)**, que é a substituição oficial e recomendada.

---

## Etapa 1 — Preparar o ambiente administrativo

**Objetivo:** garantir que você tem permissões e que a organização está configurada para governança.

### 1.1 Validar suas permissões

- [ ] Acessar https://admin.microsoft.com com sua conta corporativa
- [ ] No menu lateral, ir em **Funções → Atribuições da função** e confirmar que sua conta tem **Administrador do Teams** ou **Administrador Global**
- [ ] Caso não tenha, solicitar ao responsável pela conta Microsoft 365 da IANorth

### 1.2 Restringir criação de Equipes (governança)

Por padrão, qualquer usuário do M365 pode criar uma Equipe nova. Isso gera proliferação descontrolada — exatamente o que a Seção 9 das Convenções proíbe. Vamos restringir.

- [ ] Acessar https://admin.microsoft.com
- [ ] Ir em **Configurações → Configurações da organização → Microsoft 365 Groups**
- [ ] Marcar **"Permitir que membros criem grupos"** como **Desativado** ou restringir a um grupo específico
- [ ] Criar um grupo de segurança chamado `G-Teams-Creators` no Entra ID
- [ ] Adicionar a esse grupo apenas: você (Direção Técnica) e eventualmente um suplente

> 💡 Esta etapa exige PowerShell para configuração completa. Execute o script abaixo ou solicite a um admin de TI:
>
> ```powershell
> Connect-MgGraph -Scopes "Directory.ReadWrite.All"
> $GroupName = "G-Teams-Creators"
> $AllowGroupCreation = "False"
> $settingsObjectID = (Get-MgDirectorySetting | Where-Object -Property DisplayName -Value "Group.Unified" -EQ).Id
> # ... configurar template Group.Unified
> ```
>
> Documentação completa: https://learn.microsoft.com/en-us/microsoft-365/solutions/manage-creation-of-groups

### 1.3 Configurar política de naming

- [ ] No mesmo painel, ativar **Política de nomenclatura de grupos**
- [ ] Definir prefixo padrão: `IANorth-` para Equipes institucionais e `Projeto-` para Equipes de projeto
- [ ] Adicionar palavras bloqueadas (ex.: nomes de clientes em texto livre — Sinobras deve usar codinome interno)

---

## Etapa 2 — Criar a estrutura de Equipes

**Objetivo:** instanciar as três Equipes principais conforme arquitetura definida nas Convenções.

### 2.1 Equipe Institucional

- [ ] Abrir o Teams (cliente desktop ou web em https://teams.microsoft.com)
- [ ] Clicar em **Equipes** no menu lateral → **Ingressar ou criar equipe → Criar equipe → Do zero**
- [ ] Escolher **Privada** (apenas membros aprovados acessam)
- [ ] Nome: `IANorth — Geral`
- [ ] Descrição: `Comunicação institucional, avisos e cultura da IANorth Tecnologia`
- [ ] Adicionar todos os colaboradores ativos como membros
- [ ] Definir **você como Owner** e mais um suplente

### 2.2 Equipe Engenharia

- [ ] Repetir o processo
- [ ] Nome: `IANorth — Engenharia`
- [ ] Descrição: `Discussões técnicas transversais entre projetos`
- [ ] Adicionar apenas membros do time técnico (devs CLT, PJs com escopo técnico)
- [ ] **Não adicionar** estagiários ainda — eles entram conforme Fase 1 do onboarding

### 2.3 Equipes de Projeto

Para cada projeto ativo, criar uma Equipe separada. Exemplo:

- [ ] `Projeto — Sinobras App 1`
- [ ] `Projeto — Sinobras App 2`
- [ ] `Projeto — Sinobras App 3`
- [ ] `Projeto — Sinobras App 4`

Em cada uma:

- [ ] Tipo **Privada**
- [ ] Adicionar apenas membros alocados àquele projeto específico (princípio do menor privilégio aplicado à comunicação)
- [ ] Owner: você + responsável técnico daquele projeto, se houver

---

## Etapa 3 — Criar canais dentro de cada Equipe

**Objetivo:** estruturar os subtemas conforme padrão das Convenções.

### 3.1 Canais da Equipe `IANorth — Geral`

Em cada criação: clicar nos `...` ao lado do nome da Equipe → **Adicionar canal**.

- [ ] `📢 anuncios` — Padrão. Configurar em Permissões: **Apenas Owners podem postar**
- [ ] `☕ random` — Padrão
- [ ] `📚 aprendizado` — Padrão

### 3.2 Canais da Equipe `IANorth — Engenharia`

- [ ] `💬 dev-geral` — Padrão
- [ ] `🔍 code-review` — Padrão
- [ ] `🚨 incidentes` — Padrão. Configurar em Permissões: postagem livre
- [ ] `🚀 deploys-producao` — Padrão
- [ ] `🤖 alertas-ci` — Padrão
- [ ] `🙋 duvidas-juniores` — Padrão. Cultura: postagem livre, zero julgamento

### 3.3 Canais de cada Equipe `Projeto — {Nome}`

- [ ] `📋 geral` — Padrão. Configurar em Permissões: **Apenas Owners podem postar**
- [ ] `💻 dev` — Padrão
- [ ] `📦 commits-prs` — Padrão
- [ ] `🐛 bugs` — Padrão
- [ ] `📐 arquitetura` — Padrão
- [ ] `🎧 pair-programming` — Padrão. Sala para Meet Now ad-hoc

> 💡 **Dica de produtividade:** depois de configurar a primeira Equipe de projeto completa, você pode usar a Etapa 9 deste guia para transformá-la em **modelo (template)** e reutilizar nas próximas.

---

## Etapa 4 — Configurar permissões e canal somente-leitura

**Objetivo:** transformar canais de avisos em "muros institucionais" e proteger o canal Geral.

Para cada canal que precisa ser somente-leitura (`📢 anuncios`, `📋 geral` de cada projeto):

- [ ] Clicar nos `...` ao lado do nome do canal → **Gerenciar canal**
- [ ] Aba **Permissões**
- [ ] Marcar **"Apenas proprietários podem postar mensagens"**
- [ ] Salvar

---

## Etapa 5 — Criar tags para mencionar grupos

**Objetivo:** habilitar `@backend`, `@frontend`, etc., como o `@role` do Discord.

Em cada Equipe:

- [ ] Clicar nos `...` ao lado do nome da Equipe → **Gerenciar tags**
- [ ] Clicar em **Criar tag**

### Tags da Equipe `IANorth — Engenharia`

- [ ] `backend` — adicionar todos os devs backend
- [ ] `frontend` — adicionar todos os devs frontend
- [ ] `iot` — adicionar devs com responsabilidade IoT
- [ ] `juniores` — adicionar devs em Fase 1 ou 2 do onboarding
- [ ] `plantao` — atualizar semanalmente conforme escala

### Tags por Equipe de projeto

- [ ] `core-team` — devs alocados full time no projeto
- [ ] `apoio` — devs de outras equipes que prestam suporte pontual

---

## Etapa 6 — Instalar e configurar apps essenciais

**Objetivo:** equipar cada Equipe com a stack de apps padrão.

Em cada Equipe:

### 6.1 Instalar GitHub

- [ ] Clicar em **Apps** no menu lateral do Teams
- [ ] Buscar **"GitHub"** (autoria oficial *GitHub*)
- [ ] Clicar em **Adicionar** → escolher **Adicionar a uma equipe** → selecionar a Equipe
- [ ] Escolher canal padrão (`💻 dev` ou `📦 commits-prs`)

### 6.2 Instalar Planner / Tasks

- [ ] Apps → buscar **"Tasks by Planner and To Do"**
- [ ] Adicionar à Equipe
- [ ] Criar plano inicial: `Backlog — {Nome do Projeto}`

### 6.3 Instalar Loop

- [ ] Apps → buscar **"Loop"**
- [ ] Adicionar como aba em canais de discussão técnica (`📐 arquitetura`)

### 6.4 Instalar Approvals

- [ ] Apps → buscar **"Approvals"**
- [ ] Adicionar como app pessoal e à Equipe Engenharia
- [ ] Útil para: aprovação de deploys, mudanças de arquitetura, decisões de PR críticos

### 6.5 Instalar Forms

- [ ] Apps → buscar **"Forms"**
- [ ] Adicionar à Equipe `IANorth — Geral`
- [ ] Será usado para os fluxos de aceite de NDA (Playbook Seção 5)

---

## Etapa 7 — Configurar GitHub for Teams (notificações de repositório)

**Objetivo:** receber notificações de PRs, commits, issues e deploys nos canais corretos.

### 7.1 Autenticação

- [ ] No canal `💻 dev` da Equipe de projeto, clicar em **Nova conversa**
- [ ] Digitar `@github signin` e enviar
- [ ] Seguir o fluxo OAuth para autorizar a integração com a org `IANorth-Tecnologia`

### 7.2 Subscriptions por canal

Em cada canal de projeto, executar os comandos correspondentes:

**Canal `💻 dev`** (visão geral do repositório):
```
@github subscribe IANorth-Tecnologia/sinobras-app1 issues pulls commits releases deployments
```

**Canal `📦 commits-prs`** (apenas commits e PRs):
```
@github subscribe IANorth-Tecnologia/sinobras-app1 commits pulls
```

**Canal `🐛 bugs`** (apenas issues com label `bug`):
```
@github subscribe IANorth-Tecnologia/sinobras-app1 issues:label:"bug"
```

**Canal `🚀 deploys-producao` (Equipe Engenharia)**:
```
@github subscribe IANorth-Tecnologia/sinobras-app1 releases deployments
@github subscribe IANorth-Tecnologia/sinobras-app2 releases deployments
@github subscribe IANorth-Tecnologia/sinobras-app3 releases deployments
```

**Canal `🔍 code-review` (Equipe Engenharia)**:
```
@github subscribe IANorth-Tecnologia/sinobras-app1 pulls
```
Repetir para cada repositório.

### 7.3 Verificar funcionamento

- [ ] Abrir um PR de teste em um repositório
- [ ] Confirmar que a notificação chegou no canal correto
- [ ] Comentar no PR pelo Teams (`@github` permite comentários inline)

> 💡 Documentação oficial: https://github.com/integrations/microsoft-teams

---

## Etapa 8 — Configurar Workflow Webhook para integrações customizadas

**Objetivo:** criar endpoints HTTP que recebem POST e publicam no canal — substituto oficial dos antigos Incoming Webhooks. Útil para:

- Alertas de monitoramento (Datadog, Grafana, Azure Monitor)
- Notificações de pipelines CI/CD não-GitHub
- Alertas de aplicações em produção (Sinobras)
- Integração com sistemas internos da IANorth

### 8.1 Criar o fluxo a partir do canal

- [ ] Ir até o canal onde quer receber as notificações (ex.: `🚨 incidentes`)
- [ ] Clicar nos `...` ao lado do nome do canal → **Workflows**
- [ ] Buscar o template **"Post to a channel when a webhook request is received"**
- [ ] Clicar em **Próximo**

### 8.2 Configurar o fluxo

- [ ] Confirmar a conta Microsoft 365 conectada
- [ ] Confirmar a Equipe e o canal de destino
- [ ] Clicar em **Adicionar fluxo de trabalho**
- [ ] **Copiar o webhook URL gerado** — guarde com segurança (é uma URL secreta com token de autenticação embutido)

### 8.3 Testar o webhook

Use cURL no terminal para validar:

```bash
curl -X POST "<URL_DO_WEBHOOK>" \
  -H "Content-Type: application/json" \
  -d '{
    "type": "message",
    "attachments": [{
      "contentType": "application/vnd.microsoft.card.adaptive",
      "content": {
        "type": "AdaptiveCard",
        "version": "1.4",
        "body": [{
          "type": "TextBlock",
          "text": "✅ Teste de webhook funcionando!",
          "weight": "Bolder",
          "size": "Medium"
        }]
      }
    }]
  }'
```

- [ ] Confirmar que a mensagem apareceu no canal correto

### 8.4 Documentar o webhook

- [ ] No SharePoint, criar planilha `Webhooks Ativos — IANorth.xlsx` com colunas: nome, canal de destino, finalidade, URL (criptografada via Azure Key Vault), data de criação, owner
- [ ] **Nunca** comitar URLs de webhook em repositórios — armazenar em Azure Key Vault
- [ ] Revisar trimestralmente quais webhooks estão ativos

> ⚠️ **Atenção:** webhooks ficam vinculados ao usuário que criou o fluxo. Se essa pessoa sair da empresa, o fluxo vira *orphan flow*. Sempre adicionar **co-owner** do fluxo via portal Power Automate (https://make.powerautomate.com).

---

## Etapa 9 — Salvar Equipe modelo para reutilização

**Objetivo:** transformar a primeira Equipe de projeto configurada em template para acelerar criação das próximas.

- [ ] Acessar o **Centro de administração do Teams** em https://admin.teams.microsoft.com
- [ ] Ir em **Equipes → Modelos de equipe**
- [ ] Clicar em **Adicionar → Criar a partir de uma equipe existente**
- [ ] Selecionar `Projeto — Sinobras App 1` (ou a primeira que você configurou completa)
- [ ] Nome do template: `Template-Projeto-IANorth`
- [ ] Descrição: `Modelo padrão para Equipes de projeto na IANorth Tecnologia`
- [ ] Revisar canais e apps incluídos
- [ ] Salvar

A partir daí, criar uma nova Equipe de projeto leva 30 segundos: **Criar equipe → Selecionar template → Template-Projeto-IANorth**.

---

## Etapa 10 — Configurar políticas de notificação recomendadas

**Objetivo:** estabelecer perfil de notificação saudável para o time evitar burnout.

> Esta etapa é **individual** — cada dev configura para si. Mas vale você fazer uma sessão de 15 minutos com cada novo membro mostrando como.

Para cada usuário:

- [ ] No Teams, clicar no avatar (canto superior direito) → **Configurações → Notificações e atividade**
- [ ] **Mensagens em chats e canais → Personalizado**

### Perfil recomendado por canal

| Canal | Notificação |
|---|---|
| `🚨 incidentes` | Todas as atividades |
| Mensagens diretas (DM) | Banner e e-mail |
| `💻 dev` (canais dos seus projetos) | Apenas menções e respostas |
| `🔍 code-review` | Apenas menções e respostas |
| `📦 commits-prs` | **Mute** |
| `🤖 alertas-ci` | **Mute** (exceto se estiver de plantão) |
| `🚀 deploys-producao` | **Mute** (exceto se estiver de plantão) |
| `☕ random` | **Mute** |
| Demais canais não relevantes | **Mute** |

---

## Etapa 11 — Testar a integração ponta a ponta

**Objetivo:** validar que tudo funciona antes de declarar pronto.

Cenário de teste:

- [ ] Criar branch de teste no repositório `IANorth-Tecnologia/sinobras-app1`
- [ ] Fazer commit pequeno
- [ ] Abrir PR
- [ ] **Verificar:** notificação aparece em `📦 commits-prs` e `🔍 code-review` ✅
- [ ] Solicitar review da Direção Técnica
- [ ] Aprovar e fazer merge
- [ ] **Verificar:** notificação de merge aparece nos canais ✅
- [ ] Criar release no GitHub
- [ ] **Verificar:** notificação chega em `🚀 deploys-producao` ✅
- [ ] Disparar webhook de teste com cURL para `🚨 incidentes`
- [ ] **Verificar:** mensagem renderiza corretamente ✅

Se todas as verificações passaram: **ambiente operacional.**

---

## Etapa 12 — Comunicar e treinar o time

**Objetivo:** garantir adoção real, não só configuração técnica.

- [ ] Postar em `📢 anuncios` (Equipe `IANorth — Geral`) o anúncio oficial da migração para Teams
- [ ] Compartilhar links: **Playbook de Onboarding**, **Convenções de Uso do Teams**, este **Guia de Configuração**
- [ ] Agendar uma sessão de 30 minutos com o time atual mostrando:
  - Estrutura de Equipes e canais
  - Como criar nova conversa com título
  - Como configurar notificações
  - Como usar tags
  - Como iniciar Meet Now em `🎧 pair-programming`
- [ ] Definir **data de corte** após a qual Discord não é mais usado para registro oficial
- [ ] Coletar feedback após 2 semanas de uso e ajustar

---

## Checklist final de implementação

Imprima ou copie esta seção como controle:

- [ ] Etapa 1 — Ambiente administrativo preparado
- [ ] Etapa 2 — Equipes criadas (Geral, Engenharia, Projetos)
- [ ] Etapa 3 — Canais criados em todas as Equipes
- [ ] Etapa 4 — Permissões de canais Geral configuradas
- [ ] Etapa 5 — Tags criadas (`backend`, `frontend`, `juniores`, etc.)
- [ ] Etapa 6 — Apps essenciais instalados em cada Equipe
- [ ] Etapa 7 — GitHub for Teams configurado em todos os repositórios
- [ ] Etapa 8 — Workflow Webhooks criados e testados
- [ ] Etapa 9 — Template de Equipe de projeto salvo
- [ ] Etapa 10 — Políticas de notificação documentadas
- [ ] Etapa 11 — Teste ponta a ponta validado
- [ ] Etapa 12 — Time comunicado e treinado

---

## Solução de problemas comuns

**`@github` não responde no canal**
→ Verificar se o app GitHub foi instalado naquela Equipe específica (instalação por Equipe, não global). Refazer Etapa 6.1.

**Webhook do Workflow retorna 401 / 403**
→ URL expirou ou o owner do fluxo perdeu acesso. Recriar fluxo na Etapa 8 e atualizar URL no sistema externo.

**Notificações do GitHub aparecem em canal errado**
→ Executar `@github unsubscribe <repo>` no canal errado e refazer subscribe no canal correto.

**Pessoa não consegue criar nova Equipe**
→ Comportamento esperado após Etapa 1.2. Solicitar criação à Direção Técnica.

**Nome de canal com emoji aparece quebrado em alguns clientes**
→ Usar emoji do conjunto Unicode padrão (não emoji custom). Os emojis sugeridos neste guia são todos compatíveis.

---

## Manutenção contínua

Tarefas recorrentes da Direção Técnica para manter o ambiente saudável:

| Frequência | Tarefa |
|---|---|
| **Semanal** | Atualizar tag `@plantao` conforme escala |
| **Quinzenal** | Revisar canais de projetos arquivados ou inativos por 60+ dias |
| **Mensal** | Revisar membros de cada Equipe vs. alocação atual de projetos |
| **Trimestral** | Auditoria completa: webhooks ativos, fluxos órfãos, permissões |
| **Semestral** | Revisão deste guia e das Convenções com base em feedback do time |

---

> **IANorth Tecnologia** — Documento operacional complementar
> Confidencial — Uso Interno
