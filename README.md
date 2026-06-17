# tonelli-claude-skills

> Uma coletânea curada de **Skills do Claude Code** que fazem sentido no meu dia a dia de desenvolvimento — organizadas por stack e prontas para instalar **globalmente** (valem para qualquer projeto) ou **localmente** (dentro de um projeto específico).

Skills são pacotes de instruções, contexto e conhecimento especializado que o [Claude Code](https://docs.claude.com/en/docs/claude-code/overview) carrega automaticamente quando a tarefa combina com a descrição da skill. Na prática, é como dar ao Claude um "manual" do seu jeito de trabalhar, da sua stack e das suas convenções — sem precisar repetir o mesmo contexto toda vez.

Este repositório reúne as skills que uso de verdade, agrupadas em sete frentes:

- 🌍 **Opcionais** — fluxo de trabalho (planejar, depurar, revisar, finalizar) — instalação manual, não vêm habilitadas por padrão
- 🟦 **Advpl** — desenvolvimento TOTVS Protheus (AdvPL / TLPP)
- ⚛️ **React / Next** — frontend moderno com React e Next.js
- 🐦 **Flutter** — aplicações cross-platform com Flutter e Dart
- 🟩 **Node.js** — APIs, especificações OpenAPI e otimização SQL
- 🔁 **n8n** — automação de workflows e integrações low-code
- 🧑‍💻 **Pessoais** — minhas skills autorais

---

## 📑 Sumário

- [tonelli-claude-skills](#tonelli-claude-skills)
  - [📑 Sumário](#-sumário)
  - [🧠 Como as Skills funcionam](#-como-as-skills-funcionam)
  - [🚀 Instalação](#-instalação)
    - [Instalação Global](#instalação-global)
    - [Instalação Local (por projeto)](#instalação-local-por-projeto)
  - [📜 CLAUDE.md global](#-claudemd-global)
  - [📦 Skills disponíveis](#-skills-disponíveis)
    - [🌍 Opcionais — Fluxo de trabalho](#-opcionais--fluxo-de-trabalho)
    - [🟦 Advpl — TOTVS Protheus (AdvPL / TLPP)](#-advpl--totvs-protheus-advpl--tlpp)
    - [⚛️ React / Next — Frontend](#️-react--next--frontend)
    - [🐦 Flutter — Mobile / Desktop](#-flutter--mobile--desktop)
    - [🟩 Node.js — Backend](#-nodejs--backend)
    - [🔁 n8n — Automação de Workflows](#-n8n--automação-de-workflows)
    - [🧑‍💻 Pessoais](#-pessoais)
  - [🗂 Estrutura do repositório](#-estrutura-do-repositório)
  - [🙏 Créditos e fontes](#-créditos-e-fontes)
  - [🤝 Contribuindo](#-contribuindo)
  - [📄 Licença](#-licença)

---

## 🧠 Como as Skills funcionam

Cada skill é uma pasta contendo um arquivo `SKILL.md` com um *frontmatter* (`name` + `description`) e o corpo com as instruções. O Claude Code lê a `description` de todas as skills instaladas e **decide sozinho** quando ativar uma delas, com base no que você está pedindo. Você também pode invocá-las manualmente com `/nome-da-skill`.

```text
minha-skill/
├── SKILL.md          # frontmatter (name, description) + instruções
├── reference.md      # (opcional) material de apoio carregado sob demanda
└── ...
```

Quanto mais específica a `description`, melhor o Claude acerta o momento de usar a skill.

---

## 🚀 Instalação

> Pré-requisito: ter o [Claude Code](https://docs.claude.com/en/docs/claude-code/overview) instalado.

Há dois lugares onde as skills podem viver:

### Instalação Global (opcional)

Skills globais ficam disponíveis em **todos os projetos** da sua máquina. As de fluxo de trabalho (planejar, depurar, revisar etc.) ficam em `Global/opicionais/` e são **opcionais** — não vêm habilitadas por padrão. Instale manualmente apenas as que quiser.

**Local de instalação (Windows):**

```text
C:\Users\<seu-usuario>\.claude\skills
```

No meu caso:

```text
C:\Users\raphael.tonelli\.claude\skills
```

Basta copiar a pasta da skill para dentro desse diretório:

```powershell
# Exemplo: instalar uma skill opcional específica
Copy-Item -Recurse .\Global\opicionais\systematic-debugging "$env:USERPROFILE\.claude\skills\"

# Ou todas as opcionais de uma vez
Copy-Item -Recurse .\Global\opicionais\* "$env:USERPROFILE\.claude\skills\"
```

> 💡 Em macOS/Linux o caminho equivalente é `~/.claude/skills`.

### Instalação Local (por projeto)

Skills locais ficam **dentro do projeto** e são versionadas junto com ele — perfeito para compartilhar com o time e amarrar a skill à stack daquele repositório. Use estas para as skills de **Advpl**, **React/Next** etc., conforme a stack do projeto.

**Local de instalação:**

```text
<raiz-do-projeto>\.claude\skills
```

```powershell
# Exemplo: instalar as skills de React no projeto atual
New-Item -ItemType Directory -Force ".\.claude\skills" | Out-Null
Copy-Item -Recurse "C:\caminho\para\tonelli-claude-skills\React\skills\*" ".\.claude\skills\"
```

> 💡 Como ficam dentro de `.claude/skills` no projeto, podem ser commitadas no Git e o time inteiro passa a ter o mesmo contexto.

---

## 📜 CLAUDE.md global

Além das skills, este repositório traz um [`CLAUDE.md`](CLAUDE.md) — um arquivo de **diretrizes de comportamento** que o Claude Code carrega automaticamente como contexto. Enquanto as skills entregam conhecimento sob demanda, o `CLAUDE.md` define *como* o Claude deve trabalhar em **todos os meus projetos**: pensar antes de codar, manter simplicidade, fazer mudanças cirúrgicas, verificar antes de afirmar e — importante — usar as skills **só quando a tarefa realmente exigir**.

Ele também ensina ao Claude o **catálogo das minhas skills** (globais e por stack), deixando claro que elas são opcionais: se uma skill não estiver instalada no projeto, o desenvolvimento segue normalmente sem ela.

**Principais diretrizes:**

- 🧠 **Pensar antes de codar** — explicitar suposições, apontar interpretações ambíguas e sugerir abordagens mais simples.
- ✂️ **Mudanças cirúrgicas** — editar só o que o pedido exige; não mexer em código não relacionado nem ao fluxo.
- 🪶 **Simplicidade primeiro** — o mínimo de código que resolve; nada especulativo. A maioria das tarefas é simples.
- ✅ **Evidência antes de afirmar** — não dizer "pronto/corrigido" sem verificar a saída.
- 🎨 **Respeitar o padrão do projeto** — em React/Next, seguir o que o projeto já usa (Tailwind, shadcn/ui ou CSS manual).
- 🔤 **Encoding Protheus** — fontes AdvPL/TLPP sempre em Windows-1252 (CP1252).
- 🧩 **Skills sob demanda** — `plan`/`debug` só quando solicitados; `/commit` e `/merge` quando chamados; code review para conferir mudanças.

**Instalação (global, vale para todos os projetos):**

> É possível usá-lo **por projeto**: basta colocar uma cópia (ajustada às necessidades do repositório) na raiz do projeto. Quando há um `CLAUDE.md` no projeto, as instruções dele têm prioridade sobre as globais.

---

## 📦 Skills disponíveis

### 🌍 Opcionais — Fluxo de trabalho

Skills de processo de desenvolvimento, agnósticas de stack. Vivem em `Global/opicionais/` e são **opcionais**: não vêm mais habilitadas por padrão — instale manualmente (global ou local) só quando quiser. Vêm do excelente projeto [obra/superpowers](https://github.com/obra/superpowers).

| Skill                            | O que faz                                                                                                                                            |
| -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| `writing-plans`                  | Transforma uma spec ou conjunto de requisitos em um plano de implementação detalhado **antes** de tocar no código.                                   |
| `executing-plans`                | Executa um plano de implementação já escrito, em sessão separada e com checkpoints de revisão.                                                       |
| `systematic-debugging`           | Aplica um método sistemático de depuração diante de qualquer bug, teste falhando ou comportamento inesperado — **antes** de propor correções.        |
| `verification-before-completion` | Exige rodar comandos de verificação e confirmar a saída **antes** de afirmar que algo está pronto/corrigido/passando. Evidência antes de afirmações. |
| `requesting-code-review`         | Conduz uma revisão de código ao concluir tarefas ou antes de fazer merge, garantindo que o trabalho atende aos requisitos.                           |
| `receiving-code-review`          | Orienta a **recepção** de feedback de code review: verificar antes de implementar, perguntar antes de assumir, fazer pushback técnico quando necessário — sem concordância performática.  |
| `finishing-a-development-branch` | Ajuda a finalizar uma branch de desenvolvimento apresentando opções estruturadas de merge, PR ou limpeza.                                            |
| `careful`                        | Ativa **guardrails de segurança** para comandos destrutivos (`rm -rf`, `DROP TABLE`, `git push --force`, `kubectl delete` etc.): emite aviso antes de cada operação de risco. Use com "be careful", "safety mode" ou "prod mode". |

**Fonte original:** [github.com/obra/superpowers](https://github.com/obra/superpowers) · [gstack](https://github.com/gstack) (`careful`)

---

### 🟦 Advpl — TOTVS Protheus (AdvPL / TLPP)

Skills focadas no desenvolvimento de customizações no ERP **TOTVS Protheus**. **Recomendadas para instalação local**, nos projetos AdvPL/TLPP.

| Skill                          | O que faz                                                                                                                                                                                                                                                      |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `advpl-to-tlpp-migration`      | Guia a migração de código legado **AdvPL → TLPP**: comparação de recursos, transformação de sintaxe, includes/namespaces, tipagem, Try-Catch, REST, identificadores longos, JSON inline, parâmetros nomeados, modificadores de acesso e remoção de StaticCall. |
| `mvc-generator`                | Gera estruturas de tela **MVC do Protheus** (`ModelDef`, `ViewDef`, `MenuDef`, `BrowseDef`) com FWFormModel/View/Browse, validações, gatilhos e hooks — Modelo 1 (entidade única) e Modelo 3 (master-detail).                                                  |
| `tlpp-rest-endpoint-generator` | Gera endpoints **REST em TLPP** com roteamento por anotações (`@Get`, `@Post`, `@Put`, `@Patch`, `@Delete`) e o objeto `oRest`, seguindo o padrão TOTVS (TTALK): paginação, modelo de erro, headers padrão e Swagger.                                          |
| `fwrest-client-generator`      | Gera código AdvPL/TLPP que **consome APIs REST externas** via classe `FWRest`: verbos HTTP, headers, parâmetros, JSON, autenticação (Basic, Bearer/JWT, OAuth 2.0), timeout, SSL e tratamento de erros.                                                        |
| `entry-point-designer`         | Projeta e documenta **Pontos de Entrada** do Protheus (assinatura de `User Function`, layout de `PARAMIXB`, retornos e ProtheusDOC). Gera TLPP por padrão; AdvPL apenas quando solicitado.                                                                     |
| `query-builder`                | Monta queries **SQL seguras e otimizadas** para tabelas Protheus: filtros obrigatórios (`D_E_L_E_T_`, filial), índices sugeridos via SIX, versões Embedded SQL (`FWExecStatement`) e Workarea (`DBSelectArea`/`DBSeek`), com alertas de armadilhas comuns.     |
| `data-dictionary-lookup`       | Consulta o **dicionário de dados** do Protheus (SX2, SX3, SIX, SX6, SX5, SX7, SX1, SX9, SXB, SXG/SXA): campos, índices, parâmetros, gatilhos, relacionamentos e validação de impacto em refatorações/migrações.                                                |
| `sql-optimization`             | Otimização **universal de performance SQL** (PostgreSQL, SQL Server, Oracle): análise de plano de execução, estratégias de indexação, paginação, operações em lote e monitoramento.                                                                            |
| `code-review`                  | Revisão de código **AdvPL/TLPP** cobrindo regras SonarQube, documentação Protheus.doc, segurança, performance, clean code e boas práticas do framework Protheus.                                                                                               |
| `refactor`                     | Refatoração cirúrgica para melhorar manutenibilidade **sem mudar comportamento**: extração de funções, renomeações, quebra de "god functions", tipagem e eliminação de code smells.                                                                            |
| `context-map`                  | Gera um **mapa de contexto** dos arquivos relevantes a uma mudança antes de implementar: arquivos a alterar, dependências, testes e padrões de referência.                                                                                                     |
| `utf8-to-cp1252-conversion`    | Converte arquivos-fonte AdvPL/TLPP de **UTF-8 para Windows-1252 (CP1252)** — encoding exigido pelo compilador Protheus. Essencial após qualquer geração de código por IA.                                                                                      |

**Fonte original:** [skills.engpro.totvs.io](https://skills.engpro.totvs.io/)

---

### ⚛️ React / Next — Frontend

Skills de boas práticas de frontend moderno. **Recomendadas para instalação local**, em projetos React/Next.js. A pasta `Next/` é um superconjunto da `React/` (inclui as mesmas skills + `next-best-practices`).

| Skill                                                  | O que faz                                                                                                                                                                                                                                           | React | Next  |
| ------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :---: | :---: |
| `building-components`                                  | Guia para construir componentes de UI modernos, acessíveis e componíveis: acessibilidade (ARIA, teclado, foco), APIs componíveis (slots, render props, estado controlado/não-controlado), design tokens, publicação em npm/registry e documentação. |   ✅   |   ✅   |
| `composition-patterns` (`vercel-composition-patterns`) | Padrões de **composição em React** que escalam: compound components, render props, context providers — incluindo mudanças de API do React 19. Ideal para acabar com a proliferação de props booleanas.                                              |   ✅   |   ✅   |
| `react-best-practices` (`vercel-react-best-practices`) | Guidelines de **performance** de React/Next.js da engenharia da Vercel: padrões para componentes, data fetching e otimização de bundle.                                                                                                             |   ✅   |   ✅   |
| `web-design-guidelines`                                | Revisa código de UI quanto às **Web Interface Guidelines**: acessibilidade, UX e boas práticas de design ("review my UI", "check accessibility").                                                                                                   |   ✅   |   ✅   |
| `next-best-practices`                                  | Boas práticas específicas de **Next.js**: file conventions, fronteiras RSC, padrões de dados, async APIs, metadata, error handling, route handlers, otimização de imagem/fonte e bundling.                                                          |   —   |   ✅   |
| `next-cache-components`                                | **Next.js 16+ Cache Components** (PPR): diretiva `use cache`, perfis `cacheLife`, invalidação com `cacheTag`/`updateTag`/`revalidateTag`, migração de `unstable_cache` e limitações de runtime.                                                    |   —   |   ✅   |
| `next-upgrade`                                         | **Upgrade incremental do Next.js**: detecta a versão atual, busca o guia oficial, roda codemods automáticos (`@next/codemod`) e orienta mudanças manuais de breaking changes para cada major.                                                       |   —   |   ✅   |
| `component-refactoring`                                | Refatora componentes React de **alta complexidade** no Dify frontend: extração de hooks, sub-componentes, context providers e lógica de formulários — acionado quando complexity > 50 ou lineCount > 300.                                           |   ✅   |   ✅   |
| `shadcn`                                               | Gerencia **shadcn/ui** em projetos existentes: adicionar, buscar, corrigir, estilizar e compor componentes; aplica/troca presets; orienta CLI, registries e regras críticas de composição.                                                          |   ✅   |   ✅   |

**Fonte original:** [Vercel — Agent Resources / Skills](https://vercel.com/docs/agent-resources/skills) · [shadcn/ui](https://ui.shadcn.com) (`shadcn`)

---

### 🐦 Flutter — Mobile / Desktop

Skills para desenvolvimento cross-platform com Flutter e Dart. **Recomendadas para instalação local**, em projetos Flutter.

| Skill                      | O que faz                                                                                                                                                                                                                                                  |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `flutter-expert`           | Engenheiro Flutter sênior para apps Flutter 3.19+ e Dart 3.3+: gerenciamento de estado com Riverpod 2.0 ou Bloc, navegação com GoRouter, widgets com `const` optimization, testes e profiling de performance.                                              |
| `flutter-init`             | Cria um **novo projeto Flutter** com Clean Architecture, Riverpod 3.0, Drift e stack moderna: escolha de domínio (Todo/Habit/Note/Expense/Custom), preset (Minimal/Essential/Full Stack), geração de código e validação com `flutter analyze`.             |
| `flutter-building-layouts` | Constrói **layouts Flutter** usando o sistema de constraints: Row/Column, Expanded/Flexible, Stack/Positioned, SizedBox, design responsivo com LayoutBuilder e resolução de erros de unbounded constraints.                                                 |
| `flutter-caching-data`     | Implementa **estratégias de cache** em apps Flutter: offline-first com repositório (stream local → fetch remoto), SQLite via sqflite, SharedPreferences, cache de imagens (`cached_network_image`), scroll cache e pre-warming do FlutterEngine no Android. |
| `flutter-dart-code-review` | Checklist de **code review Flutter/Dart** agnóstico de biblioteca: linguagem Dart, decomposição de widgets, gerenciamento de estado (BLoC, Riverpod, Provider, GetX, MobX, Signals), performance, testes, acessibilidade, segurança e i18n.                |

**Fonte original:** [github.com/Jeffallan](https://github.com/Jeffallan) (`flutter-expert`) · comunidade (demais)

---

### 🟩 Node.js — Backend

Skills para design e documentação de APIs e otimização de banco de dados. **Recomendadas para instalação local**, em projetos Node.js/backend.

| Skill                      | O que faz                                                                                                                                                                                                                                     |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `api-designer`             | Arquiteto de API sênior para **REST e GraphQL**: modelagem de recursos, padrões de URI, versionamento, paginação, error responses (RFC 7807), geração de **especificação OpenAPI 3.1** completa, autenticação e deprecation policies.         |
| `openapi-spec-generator`   | Geração automatizada de **especificações OpenAPI**: boas práticas de API Development (REST, GraphQL, autenticação), código e configurações prontos para produção, validados contra padrões OpenAPI.                                            |
| `sql-optimization-patterns`| Otimização de queries SQL e estratégias de indexação: análise de plano de execução (EXPLAIN), índices B-Tree/GIN/GiST/BRIN, eliminação de N+1, paginação por cursor, CTEs, materialized views, particionamento e monitoramento de slow queries. |

**Fonte original:** [github.com/Jeffallan](https://github.com/Jeffallan) (`api-designer`) · [Jeremy Longshore](https://github.com/intentsolutions) (`openapi-spec-generator`) · comunidade (`sql-optimization-patterns`)

---

### 🔁 n8n — Automação de Workflows

Skills para construção, configuração e depuração de workflows no **n8n**. **Recomendadas para instalação local**, em projetos que utilizam n8n como plataforma de automação.

| Skill                       | O que faz                                                                                                                                                                                                                                                     |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `n8n-workflow-patterns`     | **6 padrões arquiteturais** prontos para uso: webhook processing, HTTP API integration, database operations, AI agent workflow, scheduled tasks e batch processing (SplitInBatches, loops aninhados, paginação). Inclui checklist completo de criação de workflow. |
| `n8n-expression-syntax`     | Sintaxe de **expressões `{{ }}`**: variáveis `$json`, `$node`, `$now`, `$env`; acesso a dados de webhook (armadilha do `.body`); erros comuns e quando NÃO usar expressões (Code nodes, credenciais, paths).                                                  |
| `n8n-node-configuration`    | Configuração **orientada a operação**: dependências de propriedades (`displayOptions`), níveis de detalhe do `get_node` (standard/full/search), padrões por tipo de nó (resource-operation, HTTP, database, conditional) e edições cirúrgicas com `patchNodeField`. |
| `n8n-validation-expert`     | Interpretação e correção de **erros de validação**: perfis (minimal/runtime/ai-friendly/strict), auto-sanitização de operadores, falsos positivos, `n8n_autofix_workflow` e estratégias de recuperação.                                                        |
| `n8n-code-javascript`       | Código **JavaScript em Code nodes**: sintaxe `$input/$json/$node`, `$helpers.httpRequest`, DateTime/Luxon, `$jmespath`, SplitInBatches com acumulação cross-iteration via `$getWorkflowStaticData`, `pairedItem` e os 5 erros mais comuns.                    |
| `n8n-code-python`           | Código **Python (Beta) em Code nodes**: sintaxe `_input/_json/_node`, apenas biblioteca padrão (sem `requests`/`pandas`), diferenças entre Python Beta e Native, e guia de quando preferir JavaScript.                                                        |
| `n8n-code-tool`             | **Code Tool para AI Agents** (`@n8n/n8n-nodes-langchain.toolCode`): retorna string (não array de itens), lê input via `query`, schemas estruturados com `specifyInputSchema`, erros comuns (`$fromAI` indisponível, "Wrong output type") e quando usar `toolWorkflow` no lugar. |

**Fonte original:** comunidade n8n

---

### 🧑‍💻 Pessoais

Skills autorais, otimizadas para o meu fluxo. Sempre se comunicam em **português (pt-BR)**.

| Skill                                 | O que faz                                                                                                                                                                                                                                                                                                         |
| ------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `tonelli-git-commit-generator`        | Gera mensagens de commit de alta qualidade analisando o **diff real do staged** e seguindo **Conventional Commits** + versionamento semântico: infere tipo (incl. `perf`/`chore`/`revert`), escopo e descrição, detecta breaking changes e propõe um commit seguro no PowerShell. Disparada por `/commit`.        |
| `tonelli-git-merge-request-generator` | Gera descrição de **Merge/Pull Request** (`MERGE.md`) analisando o diff real entre duas branches: **detecta GitLab vs GitHub**, aceita `/merge <target>` usando a branch atual como origem, extrai card/chamado **Jira** do nome da branch, alerta quando a branch está desatualizada e detecta breaking changes. |

**Fonte original:** autorais (Raphael Tonelli).

---

## 🗂 Estrutura do repositório

```text
tonelli-claude-skills/
├── CLAUDE.md                              # → diretrizes globais (~/.claude/CLAUDE.md)
├── .gitignore                             # → ignora artefatos de IA (mantém CLAUDE.md)
│
├── Global/
│   ├── skills/                            # → vazia por padrão (instale aqui o que quiser global)
│   └── opicionais/                        # → skills de fluxo, opcionais (instalação manual)
│       ├── writing-plans/
│       ├── executing-plans/
│       ├── systematic-debugging/
│       ├── verification-before-completion/
│       ├── requesting-code-review/
│       ├── receiving-code-review/
│       ├── finishing-a-development-branch/
│       └── careful/
│
├── Advpl/skills/                   # → instalar local em projetos Protheus
│   ├── advpl-to-tlpp-migration/
│   ├── mvc-generator/
│   ├── tlpp-rest-endpoint-generator/
│   ├── fwrest-client-generator/
│   ├── entry-point-designer/
│   ├── query-builder/
│   ├── data-dictionary-lookup/
│   ├── sql-optimization/
│   ├── code-review/
│   ├── refactor/
│   ├── context-map/
│   └── utf8-to-cp1252-conversion/
│
├── React/skills/                   # → instalar local em projetos React
│   ├── building-components/
│   ├── composition-patterns/
│   ├── react-best-practices/
│   ├── web-design-guidelines/
│   ├── component-refactoring/
│   └── shadcn/
│
├── Next/skills/                    # → instalar local em projetos Next.js
│   ├── building-components/
│   ├── composition-patterns/
│   ├── react-best-practices/
│   ├── web-design-guidelines/
│   ├── next-best-practices/
│   ├── next-cache-components/
│   └── next-upgrade/
│
├── Flutter/skills/                 # → instalar local em projetos Flutter
│   ├── flutter-expert/
│   ├── flutter-init/
│   ├── flutter-building-layouts/
│   ├── flutter-caching-data/
│   └── flutter-dart-code-review/
│
├── Node/skills/                    # → instalar local em projetos Node.js/backend
│   ├── api-designer/
│   ├── openapi-spec-generator/
│   └── sql-optimization-patterns/
│
├── n8n/skills/                     # → instalar local em projetos n8n
│   ├── n8n-workflow-patterns/
│   ├── n8n-expression-syntax/
│   ├── n8n-node-configuration/
│   ├── n8n-validation-expert/
│   ├── n8n-code-javascript/
│   ├── n8n-code-python/
│   └── n8n-code-tool/
│
└── Pessoais/skills/                # → instalar global ou local
    ├── tonelli-git-commit-generator/
    └── tonelli-git-merge-request-generator/
```

---

## 🙏 Créditos e fontes

Este repositório é uma **curadoria**. O mérito das skills de terceiros é de seus autores originais — todo o crédito a eles. As principais fontes de busca e referência foram:

- **Vercel — Agent Resources / Skills** → [vercel.com/docs/agent-resources/skills](https://vercel.com/docs/agent-resources/skills)
- **obra/superpowers** → [github.com/obra/superpowers](https://github.com/obra/superpowers)
- **TOTVS — Engenharia de Produto (Skills)** → [skills.engpro.totvs.io](https://skills.engpro.totvs.io/)

As skills da pasta `Pessoais/` são de autoria própria.

---

## 🤝 Contribuindo

Sugestões, correções e novas skills são **muito bem-vindas**! Se você:

- conhece uma skill que faz sentido nessas stacks,
- tem uma melhoria para alguma skill existente, ou
- quer adaptar uma skill para outro contexto,

abra uma **issue** ou mande um **pull request**. Quanto mais gente compartilhando seus fluxos, melhor para toda a comunidade.

> Ao adicionar uma skill de terceiros, mantenha sempre o crédito e o link para o repositório original.

---

## 📄 Licença

As skills de terceiros mantêm a licença de seus repositórios de origem (em geral **MIT** — verifique o `SKILL.md` de cada uma). As skills da pasta `Pessoais/` são disponibilizadas sob licença **MIT**.

---

<div align="center">

**Curtiu? Deixe uma ⭐ no repositório e compartilhe com quem usa Claude Code!**

Feito com ☕ por [Raphael Tonelli](https://github.com/TonelliMG)

</div>
