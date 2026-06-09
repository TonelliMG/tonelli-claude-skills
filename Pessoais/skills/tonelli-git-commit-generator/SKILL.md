---
name: tonelli-git-commit-generator
description: Generate high-quality, Conventional Commit messages by analyzing the STAGED changes of a Git repository. Inspects the real diff (not filenames), infers type and scope from the actual code, detects breaking changes, and proposes a ready-to-run commit. Optimized for daily use across React, Next.js, Node, and TOTVS Protheus (AdvPL/TLPP) projects. Use when the user runs "/commit", asks to "generate a commit message", "create a commit", "commitar", or "gerar commit". All user-facing output and the commit message itself are written in Brazilian Portuguese (pt-BR); code identifiers are preserved as-is.
license: MIT
metadata:
  author: Raphael Tonelli
  version: "2.0.0"
  category: Git / Workflow
  language: pt-BR (output) / en (definition)
---

# Tonelli Git Commit Generator

Generates production-quality Git commit messages from the **staged** changes of a
repository, following Conventional Commits and Semantic Versioning. The skill
reads the actual diff, infers the most appropriate type and scope, detects
breaking changes, and proposes a ready-to-run commit — always after explicit user
confirmation.

## Trigger

Invoke with `/commit`, or when the user asks to "gerar commit", "commitar",
"create a commit", or "generate a commit message".

## Output language

- **All user-facing responses are written in Brazilian Portuguese (pt-BR).**
- **The commit message itself is written in pt-BR**, regardless of the language
  used in the repository's existing history.
- **Code identifiers are preserved verbatim** — function names, variables, file
  names, table/field names, API routes, etc. stay exactly as they appear in the
  code (typically English). Do not translate them.

Example: `feat(auth): adiciona validação no AuthService.login()` — prose in pt-BR,
`AuthService.login()` untouched.

Never produce an English sentence such as `feat(auth): add login validation`.

---

## Workflow

This is the single source of truth for execution. Follow the steps in order.

### Step 1 — Guard checks (stop early, never act blindly)

Before anything else, validate the repository state. If any guard fails, **alert
the user in pt-BR and stop — do not commit, do not stage, do not "fix" anything**:

1. **Not a Git repository** (`git rev-parse --is-inside-work-tree` fails) → alert
   that the current directory is not a Git repo and stop.
2. **Operation in progress / unresolved conflicts** — a merge, rebase, or
   cherry-pick is underway, or there are unmerged paths (`git diff
   --name-only --diff-filter=U` returns files) → alert that there is an
   unresolved conflict/operation and that the user must finish it first. Stop.
3. **Nothing staged** — there are no staged changes (`git diff --staged
   --quiet` succeeds). → Report **"não há nada preparado para commit"**. If there
   are unstaged or untracked changes, mention they exist but are **ignored on
   purpose** (this skill only commits what is staged; it never runs `git add`).
   Stop.

### Step 2 — Inspect the staged changes only

```bash
git diff --staged          # the actual content being committed
git diff --staged --stat   # file-level overview
```

This skill operates **exclusively on staged content**. Unstaged and untracked
changes are never staged or committed automatically. Base every conclusion on the
**real diff content**, never on filenames or directory names alone.

### Step 3 — Read recent history to align (not to imitate blindly)

```bash
git log --oneline -20
```

- If the recent history **already follows this skill's convention** (Conventional
  Commits in pt-BR), reuse its **scope vocabulary** for consistency.
- If the history is inconsistent, uses another style, or another language — which
  is the common case — **ignore it and apply this skill's convention**. Do not
  mimic non-conforming commit styles.
- **The generated message is always pt-BR**, no matter what the log shows.

### Step 4 — Detect project type and infer scope

Determine the stack from the staged files and repository markers, then **derive
the scope from the actual changed paths**, do not guess from a fixed list:

- Identify the **most specific common module/directory** among the staged files
  and use it as the scope (e.g. `components/Button.tsx` → `ui` or `button`;
  `src/services/auth.ts` → `auth`).
- In a monorepo, prefer the **package name** of the changed files.
- For **TOTVS Protheus**, detect the `siga*` module from the path or source and
  use it as the scope (see the scope reference below).
- If the change spans many unrelated areas, the scope may be omitted, or the
  commit should be split (see Step 7).

### Step 5 — Classify the commit type

Choose the type from the **semantic meaning of the diff**, not the file metadata.
See "Commit types" below for the full list and definitions. Always explain *why*
the chosen type fits. Prefer `perf` over `refactor` when there is a measurable
performance gain; use `refactor` only for behavior-preserving restructuring.

### Step 6 — Detect breaking changes

If the change breaks backward compatibility (removed/renamed public API, changed
signature, altered contract, incompatible schema/parameter change), mark it:

- Add `!` after the type/scope: `feat(api)!: ...`
- Add a `BREAKING CHANGE:` footer describing the impact and migration path.

### Step 7 — Compose the message

- **Subject:** `type(scope): description` — present tense, pt-BR, target **≤ 50
  characters**, hard limit **72**. No trailing period. Description starts
  lowercase.
- **Body (almost always include one):** explain **what changed and *why***, not a
  restatement of the diff. Wrap lines at **72 columns**. Only omit the body when
  the change is truly self-explanatory and a body would add nothing. Even small
  production hotfixes or quick visual tweaks should state **what was changed**.
- **Footer (when applicable):** `BREAKING CHANGE:` for breaking changes.

### Step 8 — Suggest a split if the commit is too broad

If the staged changes cover multiple unrelated logical units, propose splitting
them along logical boundaries (which files belong to each commit). Otherwise state
that a single atomic commit is appropriate.

### Step 9 — Present the proposal and ask for confirmation

Present the full proposal (see "Output format") and ask, in pt-BR:

```
Executar este commit?
[y] Sim   [n] Não
```

### Step 10 — Commit safely on confirmation

Only after the user confirms:

- Execute the commit in a way that **safely handles multi-line bodies and special
  characters in the user's shell** (PowerShell on Windows). Prefer piping the
  message via `git commit -F -` with a here-string / here-doc, or write it to a
  temporary file and use `git commit -F <file>`. Avoid a single inline `-m` with
  embedded newlines.
- **If the commit fails** (e.g. a pre-commit hook or linter rejects it), **report
  the exact error and stop. Do not retry, do not bypass hooks (`--no-verify`), and
  do not attempt any fix without first asking the user.**

---

## Commit types

| Type | When to use |
| --- | --- |
| `feat` | A new feature or user-facing capability. |
| `fix` | A bug fix. |
| `perf` | A change that improves performance (prefer over `refactor` when measurable). |
| `refactor` | Behavior-preserving restructuring; no new feature, no bug fix, no perf gain. |
| `docs` | Documentation only. |
| `style` | Formatting/whitespace/semicolons; no code-behavior change. |
| `test` | Adding or fixing tests. |
| `build` | Build system, dependencies, packaging. |
| `ci` | CI/CD configuration and pipelines. |
| `chore` | Maintenance that doesn't fit elsewhere (tooling, config, .gitignore, housekeeping). |
| `revert` | Reverts a previous commit. |

---

## Scope reference by stack

Scope must be **inferred from the changed paths** (Step 4). The lists below are
vocabulary examples per stack, not a menu to pick from blindly.

### TOTVS Protheus (AdvPL / TLPP)

Use the `siga*` module detected from the path/source as the scope. Known modules:

`sigaatf`, `sigaaud`, `sigacfg`, `sigacom`, `sigactb`, `sigaest`, `sigafat`,
`sigafin`, `sigafintms`, `sigafis`, `sigagct`, `sigagpe`, `sigajur`, `sigamdt`,
`sigamnt`, `sigaopme`, `sigapco`, `sigapon`, `sigarsp`, `sigasgq`, `sigatmk`,
`sigatms`, `sigatrm`. Generic fallback: `protheus`, `advpl`, `tlpp`.

When AdvPL/TLPP code changes, distinguish **business-rule changes** from technical
refactoring, and watch for performance improvements, SQL optimization, and
framework modernization (these affect the chosen type).

### Frontend (React, Next.js, TypeScript, JavaScript)

Example scopes: `frontend`, `ui`, `pages`, `components`, `hooks`, or the specific
component/feature directory.

### Backend (Node.js, PHP, Python)

Example scopes: `api`, `backend`, `auth`, `database`, `services`.

### Infrastructure (Docker, AWS, Kubernetes, GitHub Actions, GitLab CI)

Example scopes: `infra`, `docker`, `aws`, `ci`, `build`.

---

## Message rules

- Never produce generic messages. Forbidden: `ajustes`, `melhorias`, `correções`,
  `update stuff`, `fixes`, `miscellaneous changes`, and similar.
- Present tense, pt-BR. Subject ≤ 50 chars (hard 72); body wrapped at 72.
- Prefer atomic commits and changelog readability.
- Base the type/scope on semantic analysis of the diff, not filenames.
- Always explain why the chosen type was selected.
- Almost always include a body describing what changed and why.

---

## Output format

Respond in pt-BR with this structure:

### Resumo das alterações
Concise summary of what is staged.

### Tipo escolhido
`type(scope)` + a short justification (why this type, not another).

### Commit sugerido
```
type(scope): descrição
```

### Corpo
```
type(scope): descrição

- O que foi alterado e, principalmente, por quê.
```
(Include the body unless the change is trivially self-explanatory. Add a
`BREAKING CHANGE:` footer when applicable.)

### Possível divisão
Split strategy if the commit is too broad; otherwise state it is a single atomic
commit.

### Confirmação
```
Executar este commit?
[y] Sim   [n] Não
```

---

## Examples

### Example 1 — Feature (frontend)

```
feat(auth): adiciona autenticação multifator no login

- Adiciona fluxo de MFA com validação TOTP para reduzir o risco de
  acesso indevido em contas administrativas.
- Integra o passo de verificação ao AuthService.login() existente.
```

### Example 2 — Quick production fix (still describes the change)

```
fix(ui): corrige quebra do header em telas menores que 768px

- Ajusta o flex-wrap do menu que estourava o container no mobile,
  reportado em produção.
```

### Example 3 — Refactor (Protheus module)

```
refactor(sigatms): simplifica validações de cadastro de romaneio

- Extrai as regras repetidas para uma função auxiliar; sem mudança
  de comportamento.
```

### Example 4 — Breaking change

```
feat(api)!: padroniza envelope de resposta dos endpoints REST

- Move o payload para o campo "data" e os erros para "errors".

BREAKING CHANGE: clientes que liam a resposta na raiz precisam passar
a ler o campo "data". Atualize os consumidores antes do deploy.
```

### Example 5 — Performance

```
perf(api): reduz consultas redundantes ao banco no carregamento

- Substitui N+1 por uma consulta única com join, cortando ~70% do
  tempo de resposta da listagem.
```

---

## Safety rules (must always hold)

- Operate only on **staged** changes; never run `git add` automatically.
- On a missing repo, an in-progress operation/conflict, or nothing staged →
  **alert and do nothing**.
- Never commit without explicit user confirmation.
- On commit failure → **report and stop**; never bypass hooks or auto-fix without
  asking first.
