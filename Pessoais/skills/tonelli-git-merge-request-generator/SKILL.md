---
name: tonelli-git-merge-request-generator
description: Generate a structured Merge/Pull Request description (MERGE.md) by analyzing the real diff between two Git branches. Inspects commits, file changes, and code (not just commit messages), infers impacted areas, extracts Jira references from the branch name, detects breaking changes, and writes a ready-to-paste MERGE.md. Auto-detects GitLab (Merge Request) vs GitHub (Pull Request). Optimized for daily use across React, Next.js, Node, and TOTVS Protheus (AdvPL/TLPP) projects. Use when the user runs "/merge", asks to "gerar merge request", "gerar MR", "gerar pull request", or "criar descrição de merge". All output is written in Brazilian Portuguese (pt-BR); code identifiers are preserved as-is.
license: MIT
metadata:
  author: Raphael Tonelli
  version: "2.0.0"
  category: Git / Workflow
  language: pt-BR (output) / en (definition)
---

# Tonelli Git Merge Request Generator

Generates a structured **MERGE.md** describing a Merge Request (GitLab) or Pull
Request (GitHub), based on the **actual diff between two branches** — not just the
commit messages. The document is meant to be copied manually into GitLab/GitHub
and is usually discarded afterward.

## Trigger

```
/merge [source_branch] <target_branch>
```

Also triggers on "gerar merge request", "gerar MR", "gerar pull request",
"criar descrição de merge".

### Argument rules

- `/merge <source> <target>` — compare `source` into `target`.
- `/merge <target>` — **one argument = target**. The source is the **current
  branch** (`git rev-parse --abbrev-ref HEAD`). This avoids retyping the current
  branch name every time. Example: `/merge qa` → merge the current branch into
  `qa`.
- `/merge` with **no arguments** → **do nothing**. Reply that the command was used
  incorrectly and show the correct usage. Do not analyze, do not generate.
- More than two arguments → same as above (invalid usage, stop).

## Output language

- All user-facing responses and the entire **MERGE.md** are written in
  **Brazilian Portuguese (pt-BR)**, regardless of the language in the history.
- **Code identifiers are preserved verbatim** — function names, variables, file
  names, table/field names, API routes, Protheus identifiers — never translated.

---

## Workflow

Single source of truth for execution. Follow in order; on any failed guard,
**alert in pt-BR and stop**.

### Step 1 — Guard checks (never run without need or on a wrong state)

1. **Not a Git repository** → alert and stop.
2. **Invalid arguments** (see "Argument rules") → alert with correct usage and
   stop.
3. **Resolve branches.** Check both branches locally. If a branch is **not
   found**, run `git fetch --all` **once** (only when needed — never fetch
   otherwise) and re-check. If a branch is still missing, alert exactly which one
   and stop.
4. **Same branch** (source == target) → alert and stop.
5. **Source branch outdated** — if `target` has commits not present in `source`
   (`git rev-list --count <source>..<target>` > 0), the branch is behind. **Warn
   that the branch is outdated and ask the user to update it manually (merge or
   rebase) before generating the document. Stop — do not generate MERGE.md.**
6. **Nothing to merge** — if there are no commits in `source` beyond `target`
   (`git rev-list --count <target>..<source>` == 0), report "nada a mesclar entre
   as branches" and stop.

### Step 2 — Detect the platform

Read the remote (`git remote get-url origin` or `git config --get
remote.origin.url`):

- URL contains `gitlab` → **GitLab**: use the term **"Merge Request" (MR)**.
- URL contains `github` → **GitHub**: use the term **"Pull Request" (PR)**.
- Otherwise → default to **Merge Request** (company default) and note the
  assumption.

The output **filename is always `MERGE.md`**; only the terminology inside adapts.

### Step 3 — Analyze the branch difference

Compare what `source` introduces relative to where it diverged from `target`.
**Use the three-dot (`...`) diff on purpose**: it compares from the merge-base, so
it shows only what the source branch actually adds — not unrelated commits already
in target.

```bash
git merge-base <target> <source>                       # divergence point (explicit)
git log <target>..<source> --oneline --no-merges       # commits being merged (no merge commits)
git diff <target>...<source> --name-status             # files changed since merge-base
git diff <target>...<source> --shortstat               # files / insertions / deletions
git diff <target>...<source>                           # full diff for semantic analysis
```

Base every conclusion on the **actual diff content**, never on filenames alone.
Do not speculate about behavior that cannot be inferred from the diff; when a
conclusion is inferred, say so explicitly.

### Step 4 — Extract Jira references from the branch name

Company branches encode Jira references. Parse the **source branch** name (split
on `-`) with this generic heuristic (works across projects/teams):

- **Card Jira** = the first number that appears **immediately after the first
  text (non-numeric) segment**.
- **Chamado Jira** = the number **immediately after a `tick` segment**, when
  present.
- Any **leading number** before the first text segment is **ignored**.
- Also recognize a standard Jira key pattern `LETTERS-NUMBER` (e.g. `TMS-123`) if
  present.
- If nothing matches → "Nenhuma referência de card/chamado identificada na
  branch." Do not invent references.

Examples:

- `23-devsis-1473-melhoria` → Card Jira: **1473** (leading `23` ignored).
- `1864-devsis-1450-tick-253507-processo` → Card Jira: **1450**, Chamado Jira:
  **253507** (leading `1864` ignored).

### Step 5 — Identify impacted areas

Infer the impacted areas from the **semantic meaning** of the diff (see "Impacted
areas reference"). For Protheus, identify the affected `SIGA*` modules.

### Step 6 — Detect technical categories and breaking changes

Identify which of these the change includes: new features, bug fixes, refactoring,
performance, infrastructure, CI/CD, tests, documentation. Flag **breaking
changes** (removed/renamed public API, changed signatures, altered
contracts/schemas) for the dedicated section.

### Step 7 — Generate the MERGE.md content

Produce the document below. **The template is always the same; only the depth
adapts to the size of the change** — be concise for small changes (do not pad to
save tokens), detailed for large ones. Keep the section structure consistent.
Empty sections use their standard "Nenhuma ... identificada" line.

- **Commits list:** exclude merge commits; if there are many commits, summarize/
  group them instead of listing every one, to keep the document lean.
- **Testing recommendations:** derive them from the impacted areas, not generic
  boilerplate (e.g. an API change → suggest validating the affected endpoints).

### Step 8 — Write the file

Write the content to **`MERGE.md` at the repository root**, overwriting any
existing one (the file is disposable). **Do not print the full document in the
chat** — only confirm the path written and a one-line summary, to save tokens.

### Safety

If anything fails (git command, file write, branch resolution), **stop completely
and report the problem**. Never attempt to fix it on your own without asking
first.

---

## MERGE.md template

Adapt the title to the detected platform ("Merge Request" or "Pull Request").

```markdown
# Resumo do Merge Request

## Branch de origem
<source_branch>

## Branch de destino
<target_branch>

## Referências (Jira)
- Card Jira: <número ou "não identificado">
- Chamado Jira: <número ou "não identificado">

## Objetivo
Resumo executivo do propósito das alterações, compreensível por públicos técnico
e não técnico.

## Principais alterações
- Alteração 1
- Alteração 2

## Áreas impactadas
- Área 1
- Área 2

## Regras de negócio impactadas
Descrever as regras afetadas, quando aplicável.
(Se não houver: "Nenhuma regra de negócio impactada identificada.")

## Breaking changes
Descrever incompatibilidades e o caminho de migração, quando houver.
(Se não houver: "Nenhuma breaking change identificada.")

## Infraestrutura
Alterações de infraestrutura, pipelines ou deploy.
(Se não houver: "Nenhuma alteração de infraestrutura identificada.")

## Testes recomendados
- Teste derivado da área impactada 1
- Teste derivado da área impactada 2

## Estatísticas
- X arquivos alterados;
- Y linhas adicionadas;
- Z linhas removidas.

## Commits incluídos
- Commit 1
- Commit 2
(Se forem muitos, agrupar/resumir.)

## Observações
Informações adicionais relevantes para revisores e aprovadores.
```

---

## Impacted areas reference

Infer the scope from the actual changed paths; the lists below are vocabulary
examples per stack.

### TOTVS Protheus (AdvPL / TLPP) — use UPPERCASE module names

`SIGAATF`, `SIGAAUD`, `SIGACFG`, `SIGACOM`, `SIGACTB`, `SIGAEST`, `SIGAFAT`,
`SIGAFIN`, `SIGAFINTMS`, `SIGAFIS`, `SIGAGCT`, `SIGAGPE`, `SIGAJUR`, `SIGAMDT`,
`SIGAMNT`, `SIGAOPME`, `SIGAPCO`, `SIGAPON`, `SIGARSP`, `SIGASGQ`, `SIGATMK`,
`SIGATMS`, `SIGATRM`. Generic fallback: `PROTHEUS`, `ADVPL`, `TLPP`.

When AdvPL/TLPP changes: distinguish technical refactoring from business-rule
changes, highlight SQL optimizations, performance changes, and framework
modernization.

### Frontend (React, Next.js, TypeScript, JavaScript)

Areas: user experience, components, pages, hooks, client-side validation.

### Backend (Node.js, PHP, Python)

Areas: APIs, services, authentication, integrations, database interactions.

### Infrastructure (Docker, AWS, Kubernetes, GitHub Actions, GitLab CI)

Areas: deployment pipelines, provisioning, containerization, monitoring,
automation.

---

## Rules

- Always analyze the actual diff before generating; never rely solely on commit
  messages.
- Prioritize semantic understanding of the code changes over filenames.
- Never produce generic descriptions ("várias melhorias", "ajustes gerais",
  "correções diversas", "pequenas mudanças").
- The document must be concise but detailed enough to support review and approval.
- Do not speculate about functionality not inferable from the diff; mark inferred
  conclusions as such.
- Output is for code reviewers, testers, tech leads, and business stakeholders.
