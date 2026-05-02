# Convenções de Uso do Teams — IANorth

> Anexo ao **Playbook de Onboarding de Engenharia**.
> Este documento padroniza como o time usa o Microsoft Teams. Leitura obrigatória na Fase 1 do onboarding.

| | |
|---|---|
| **Versão** | 1.0 |
| **Última atualização** | Abril/2026 |
| **Tempo de leitura** | ~10 minutos |
| **Aplicabilidade** | Todos os colaboradores com acesso ao Teams |

---

## Por que isso importa

Ferramenta sem convenção vira ruído. O Teams permite muito — chat, threads, reuniões, arquivos, tarefas, integrações — e sem regras combinadas cada um usa de um jeito, conversas se perdem e decisões importantes ficam invisíveis.

Estas convenções existem para que **qualquer pessoa do time encontre qualquer informação relevante em menos de dois minutos**, e para que o histórico do projeto fique rastreável a longo prazo.

---

## 1. Estrutura de Equipes e Canais

### 1.1 Equipes existentes

A IANorth organiza o Teams em três tipos de Equipe:

- **🏢 Institucional** (`IANorth — Geral`) — comunicação de empresa, avisos, cultura
- **⚙️ Transversal** (`IANorth — Engenharia`) — discussões técnicas que cruzam projetos
- **🏭 Por projeto** (`Projeto — {Nome}`) — uma Equipe por projeto ativo

Cada projeto vivo tem sua própria Equipe. Projetos arquivados são marcados como **somente-leitura** após encerramento, preservando o histórico.

### 1.2 Nomenclatura de canais

Canais seguem o padrão `emoji nome-em-minusculas-com-hifens`. Exemplos válidos:

- `💻 dev`
- `🐛 bugs`
- `📐 arquitetura`
- `🎧 pair-programming`

Não usar:

- ❌ `Dev_Geral` (caixa alta, underscore)
- ❌ `desenvolvimento e arquitetura` (espaços)
- ❌ `dev` sem emoji (dificulta scan visual)

### 1.3 Canais padrão de uma Equipe de projeto

Toda nova Equipe de projeto deve ser criada com este conjunto mínimo:

| Canal | Propósito |
|---|---|
| `📋 geral` | Avisos do projeto. Apenas owners postam. |
| `💻 dev` | Discussões técnicas do dia a dia |
| `📦 commits-prs` | Notificações automáticas do GitHub |
| `🐛 bugs` | Reporte e discussão de bugs |
| `📐 arquitetura` | Decisões arquiteturais e ADRs |
| `🎧 pair-programming` | Sala de Meet Now para pareamento ad-hoc |

Canais adicionais são criados sob demanda e arquivados quando deixam de ser usados por mais de 60 dias.

---

## 2. Threads e novas conversas

### 2.1 Regra de ouro: toda conversa começa com título

Ao iniciar um tópico novo, clique em **"Nova conversa"** e escreva um título descritivo antes de mandar a mensagem. Isso transforma o canal em índice navegável.

✅ **Bons títulos:**
- `Erro de timeout no endpoint /predict em produção`
- `Decisão: migrar PostgreSQL 14 → 16`
- `Dúvida: qual padrão usamos para retry de API externa?`

❌ **Títulos ruins:**
- `ajuda`
- `pessoal`
- `pergunta rapida`

### 2.2 Responder dentro da thread, sempre

Quando alguém abrir uma conversa, suas respostas vão **dentro da thread daquele tópico** — não como nova mensagem no canal. Cortar o assunto principal com mensagens soltas é a forma mais rápida de bagunçar um canal.

### 2.3 Quando criar nova thread vs. continuar

- **Nova thread:** assunto diferente, mesmo que relacionado.
- **Continuar:** desdobramentos, dúvidas, follow-ups do mesmo tópico.

Em dúvida, prefira **nova thread**. Threads pequenas e focadas são fáceis de ler depois; threads gigantes com vários assuntos misturados são impossíveis.

---

## 3. Menções (@)

### 3.1 Hierarquia de menções

Use a menção **mais específica possível**:

| Menção | Quando usar |
|---|---|
| `@pessoa` | Quando precisa da resposta daquela pessoa especificamente |
| `@tag` (ex.: `@backend`, `@frontend`) | Quando precisa de qualquer pessoa do grupo |
| `@channel` ou `@equipe` | **Raríssimo.** Apenas avisos críticos que afetam todos. |

### 3.2 Tags disponíveis

- `@backend` — devs backend
- `@frontend` — devs frontend
- `@iot` — devs com responsabilidade em IoT
- `@plantao` — quem está de plantão na semana
- `@juniores` — devs em Fase 1 ou 2

Tags são gerenciadas pela Direção Técnica. Solicitação de nova tag via canal `📋 geral` da Equipe Engenharia.

### 3.3 Tempo de resposta esperado

| Canal | Resposta esperada |
|---|---|
| `🚨 incidentes` | Imediato (durante horário comercial) |
| `🤖 alertas-ci` | Até 1 hora útil |
| `🔍 code-review` | Até 1 dia útil |
| Demais canais técnicos | Até 2 dias úteis |
| Mensagens diretas | Até 1 dia útil |

Comunicação aqui é **assíncrona por padrão**. Não se espera resposta em minutos exceto em incidentes. Bloqueou e precisa de resposta urgente? Marca diretamente a pessoa **e** explique o motivo.

---

## 4. Notificações recomendadas

Configurações sugeridas para preservar foco:

### 4.1 Notificação completa (sempre)

- `🚨 incidentes`
- Canais onde você é owner
- Mensagens diretas (DM)

### 4.2 Apenas menções e respostas

- `💻 dev` dos projetos onde você está alocado
- `🐛 bugs` dos seus projetos
- `🔍 code-review`

### 4.3 Silenciar (mute)

- `📦 commits-prs` — automatizado, ruidoso por natureza
- `🤖 alertas-ci` — exceto se você for de plantão
- `🚀 deploys-producao` — exceto se você for de plantão
- Canais de projetos onde você não atua

> 🔕 Como configurar: clique nos `...` ao lado do canal → **Notificações de canal** → ajuste por tipo.

---

## 5. Reuniões e voz

### 5.1 Pair programming espontâneo

Use o canal `🎧 pair-programming` da Equipe do projeto. Clique em **Meet now** dentro do canal. Outros membros vêem a reunião ativa e podem entrar livremente. Equivalente ao "canal de voz" do Discord.

### 5.2 Reuniões agendadas

- Toda reunião com mais de duas pessoas tem **pauta no convite**
- Reuniões recorrentes são revisadas a cada 3 meses — sem pauta clara, são canceladas
- Gravação ativada por padrão em reuniões de decisão técnica
- Resumo escrito da decisão postado no canal apropriado **dentro de 24h** após a reunião

### 5.3 Câmera

Câmera ligada por padrão em:
- 1:1 com Direção Técnica
- Reuniões de planejamento e retrospectiva
- Apresentações ao cliente

Câmera opcional em:
- Pair programming
- Reuniões técnicas operacionais
- Daily syncs (se houver)

---

## 6. Arquivos e documentos

### 6.1 Onde mora o quê

| Tipo de conteúdo | Lugar correto |
|---|---|
| Código | GitHub |
| Documentação técnica versionada (ADRs, runbooks) | GitHub Wiki |
| Documentos vivos (atas, planilhas, briefings) | Aba Arquivos do canal Teams |
| Documentos institucionais (políticas, contratos) | SharePoint corporativo |
| Tarefas e backlog | Planner ou Azure DevOps Boards |

### 6.2 Não jogar arquivo solto no chat

Arquivos enviados pelo chat ficam difíceis de encontrar depois. Suba na **aba Arquivos** do canal e referencie pelo link na conversa. O Teams faz isso automaticamente se você arrastar pra aba certa.

### 6.3 Co-edição em vez de cópias

Word, Excel e PowerPoint editam em tempo real dentro do Teams. Não baixe, edite local e suba versão nova — quebra histórico e gera conflitos. Clique em **Editar no Teams** ou **Editar no Desktop** e edite o original.

---

## 7. Apps integrados ao Teams

Apps padrão instalados em cada Equipe:

- **GitHub** — notificações de repositório nos canais corretos
- **Workflows (Power Automate)** — para o canal `📦 commits-prs`, utilizaremos **fluxos com gatilho de webhook ("When a Teams webhook request is received")** para receber alertas de repositório em tempo real, mantendo a equipe informada sem precisar sair do Teams. Substitui os antigos *Incoming Webhooks*, descontinuados pela Microsoft em abril de 2026.
- **Planner / Tasks** — gestão de tarefas embutida
- **Loop** — colaboração em tempo real estilo Notion
- **Approvals** — fluxos de aprovação estruturados
- **Forms** — pesquisas e coleta rápida

Solicitação de novos apps via Direção Técnica. Apps de terceiros precisam de validação de segurança antes da instalação.

---

## 8. Etiqueta

Pequenos hábitos que mantêm o ambiente saudável:

- **Edite mensagens com erros** em vez de mandar correções soltas. Reduz ruído.
- **Use formatação** (negrito, listas, blocos de código) — torna mensagens longas legíveis.
- **Código vai em bloco de código** com triplo crase e linguagem (\`\`\`python). Nunca cole código direto no chat.
- **Reaja com emoji** para confirmar leitura quando uma resposta textual seria desnecessária. `👀` indica "vi, vou olhar". `✅` indica "feito".
- **GIFs e memes** são bem-vindos no `☕ random` e moderadamente em outros canais. Em `🚨 incidentes` ou `📋 geral`, evite.
- **Discordar é bom** — code review e arquitetura ganham com debate. Mas critique a ideia, nunca a pessoa. Sempre proponha alternativa quando recusar uma proposta.

---

## 9. O que NÃO fazer

Lista curta do que evita problemas reais:

- ❌ Postar credenciais, tokens, chaves de API ou secrets em qualquer canal — mesmo privado. Use o Azure Key Vault. Se algo escapar, **rotacione imediatamente** e avise em `🚨 incidentes`.
- ❌ Compartilhar dados de produção do cliente Sinobras em canais de discussão ampla.
- ❌ Adicionar pessoas externas a Equipes de projeto sem aprovação da Direção Técnica.
- ❌ **Criar novas Equipes (Teams) sem aprovação da Direção Técnica.** A proliferação descontrolada de Equipes gera silos de informação, duplicidade de canais e fragmentação do histórico do projeto.
- ❌ Apagar mensagens importantes do histórico — se errou, edite ou esclareça em resposta.
- ❌ Usar DM para decisões técnicas que afetam o time. Decisão em DM = decisão que não existiu para os outros.

---

## 10. Resumo executivo

Se você só vai ler uma seção, leia esta:

1. **Equipe = projeto. Canal = subtema.** Nomes em minúsculas com emoji e hífen.
2. **Toda conversa começa com título.** Sempre. Sem exceção.
3. **Responda dentro da thread.** Não corte assuntos com mensagens soltas no canal.
4. **Use a menção mais específica possível.** Evite `@channel`.
5. **Comunicação é assíncrona.** Resposta em minutos só em incidentes.
6. **Código em bloco de código.** Arquivos na aba Arquivos. Documentos vivos co-editados.
7. **Nunca poste secrets.** Em hipótese alguma.

---

> **IANorth Tecnologia** — Anexo ao Playbook de Onboarding de Engenharia
> Confidencial — Uso Interno
