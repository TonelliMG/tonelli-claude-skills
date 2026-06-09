# tonelli-claude-skills

> Uma coletânea curada de **Skills do Claude Code** que fazem sentido no meu dia a dia de desenvolvimento — organizadas por stack e prontas para instalar **globalmente** (valem para qualquer projeto) ou **localmente** (dentro de um projeto específico).

Skills são pacotes de instruções, contexto e conhecimento especializado que o [Claude Code](https://docs.claude.com/en/docs/claude-code/overview) carrega automaticamente quando a tarefa combina com a descrição da skill. Na prática, é como dar ao Claude um "manual" do seu jeito de trabalhar, da sua stack e das suas convenções — sem precisar repetir o mesmo contexto toda vez.

Este repositório reúne as skills que uso de verdade, agrupadas em quatro frentes:

- 🌍 **Global** — fluxo de trabalho (planejar, depurar, revisar, finalizar)
- 🟦 **Advpl** — desenvolvimento TOTVS Protheus (AdvPL / TLPP)
- ⚛️ **React / Next** — frontend moderno com React e Next.js
- 🧑‍💻 **Pessoais** — minhas skills autorais

---

## 📑 Sumário

- [tonelli-claude-skills](#tonelli-claude-skills)
  - [📑 Sumário](#-sumário)
  - [🧠 Como as Skills funcionam](#-como-as-skills-funcionam)
  - [🚀 Instalação](#-instalação)
    - [Instalação Global](#instalação-global)
    - [Instalação Local (por projeto)](#instalação-local-por-projeto)
  - [📦 Skills disponíveis](#-skills-disponíveis)
    - [🌍 Global — Fluxo de trabalho](#-global--fluxo-de-trabalho)
    - [🟦 Advpl — TOTVS Protheus (AdvPL / TLPP)](#-advpl--totvs-protheus-advpl--tlpp)
    - [⚛️ React / Next — Frontend](#️-react--next--frontend)
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

### Instalação Global

Skills globais ficam disponíveis em **todos os projetos** da sua máquina. São ideais para fluxos de trabalho que independem da stack (planejar, depurar, revisar etc.).

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
# Exemplo: instalar todas as skills globais
Copy-Item -Recurse .\Global\skills\* "$env:USERPROFILE\.claude\skills\"
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

## 📦 Skills disponíveis

### 🌍 Global — Fluxo de trabalho

Skills de processo de desenvolvimento, agnósticas de stack. **Recomendadas para instalação global.** Vêm do excelente projeto [obra/superpowers](https://github.com/obra/superpowers).

| Skill                            | O que faz                                                                                                                                            |
| -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| `writing-plans`                  | Transforma uma spec ou conjunto de requisitos em um plano de implementação detalhado **antes** de tocar no código.                                   |
| `executing-plans`                | Executa um plano de implementação já escrito, em sessão separada e com checkpoints de revisão.                                                       |
| `systematic-debugging`           | Aplica um método sistemático de depuração diante de qualquer bug, teste falhando ou comportamento inesperado — **antes** de propor correções.        |
| `verification-before-completion` | Exige rodar comandos de verificação e confirmar a saída **antes** de afirmar que algo está pronto/corrigido/passando. Evidência antes de afirmações. |
| `requesting-code-review`         | Conduz uma revisão de código ao concluir tarefas ou antes de fazer merge, garantindo que o trabalho atende aos requisitos.                           |
| `finishing-a-development-branch` | Ajuda a finalizar uma branch de desenvolvimento apresentando opções estruturadas de merge, PR ou limpeza.                                            |

**Fonte original:** [github.com/obra/superpowers](https://github.com/obra/superpowers)

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

**Fonte original:** [Vercel — Agent Resources / Skills](https://vercel.com/docs/agent-resources/skills)

---

### 🧑‍💻 Pessoais

Skills autorais, otimizadas para o meu fluxo. Sempre se comunicam em **português (pt-BR)**.

| Skill                                 | O que faz                                                                                                                                                                                                                                              |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `tonelli-git-commit-generator`        | Gera mensagens de commit de alta qualidade analisando as mudanças (staged/unstaged) e seguindo **Conventional Commits** e versionamento semântico, identificando automaticamente tipo, escopo e descrição.                                             |
| `tonelli-git-merge-request-generator` | Gera resumos de **Merge Request** analisando o diff real entre duas branches (commits, arquivos e mudanças de código) e produz um `MERGE.md` estruturado para o GitLab, com visão geral, áreas impactadas, implicações de negócio e detalhes técnicos. |

**Fonte original:** autorais (Raphael Tonelli).

---

## 🗂 Estrutura do repositório

```text
tonelli-claude-skills/
├── Global/skills/                         # → instalar globalmente (~/.claude/skills)
│   ├── writing-plans/
│   ├── executing-plans/
│   ├── systematic-debugging/
│   ├── verification-before-completion/
│   ├── requesting-code-review/
│   └── finishing-a-development-branch/
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
│   └── web-design-guidelines/
│
├── Next/skills/                    # → instalar local em projetos Next.js
│   ├── building-components/
│   ├── composition-patterns/
│   ├── react-best-practices/
│   ├── web-design-guidelines/
│   └── next-best-practices/
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
