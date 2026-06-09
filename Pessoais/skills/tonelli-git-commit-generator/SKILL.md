# Skill Name

tonelli-git-commit-generator

## Description

Generates high-quality Git commit messages based on repository changes by analyzing staged and unstaged modifications, following Conventional Commits, semantic versioning principles, and repository context.

This skill is optimized for software development workflows and automatically identifies the most appropriate commit type, scope, and description based on the actual code changes.

The skill should always communicate with the user in Brazilian Portuguese (pt-BR), regardless of the language used internally for analysis.

---

## Trigger

Primary trigger:

```text
/commit
```

---

## Behavior

When activated, the skill must:

### 1. Analyze repository status using

```bash
git status
git diff
git diff --staged
```

### 2. Detect

- Modified files
- Added files
- Deleted files
- Renamed files
- Staged changes
- Unstaged changes
- Merge conflicts
- Untracked files

### 3. Classify the change using Conventional Commits

- feat
- fix
- refactor
- docs
- style
- test
- build
- ci

### 4. Automatically infer the most appropriate scope

Examples:

- auth
- api
- frontend
- backend
- database
- ui
- infra
- docker
- aws
- advpl
- tlpp
- protheus
- sigatms
- sigafat
- sigacom

### 5. Detect

- Breaking changes
- Refactoring opportunities
- Performance optimizations
- Infrastructure changes
- CI/CD modifications

### 6. Commit quality checks

Before generating the commit message:

- Verify if there are unstaged changes.
- Verify if there are untracked files that should be included.
- Verify if there are merge conflicts.
- Verify if the commit appears too large.
- Suggest splitting commits when necessary.

### 7. If changes are too broad

Suggest splitting them into multiple commits using logical boundaries.

---

## Output Language

**IMPORTANT**

All user-facing responses MUST be written in Brazilian Portuguese (pt-BR).

Commit messages themselves MUST also be written in Brazilian Portuguese.

Example:

```text
feat(auth): adiciona autenticação multifator
```

Never generate:

```text
feat(auth): add multi-factor authentication
```

---

## Output Format

The skill must always return the following structure:

### Resumo das alterações

A concise summary of detected modifications.

### Tipo escolhido

Selected Conventional Commit type and a brief justification.

### Commit sugerido

```text
feat(scope): descrição
```

### Corpo opcional

Additional bullet points when appropriate.

Example:

```text
feat(auth): adiciona autenticação multifator

- Implementa fluxo de MFA
- Adiciona validação TOTP
- Atualiza documentação da API
```

### Possível divisão

Suggested commit split strategy if necessary.

### Comando pronto

```bash
git commit -m "feat(scope): descrição"
```

or

```bash
git commit
```

with generated title and body.

---

## Rules

- Never generate generic commit messages.
- Never use messages such as:
  - ajustes
  - melhorias
  - correções
  - update stuff
  - fixes
  - miscellaneous changes

- Keep commit summary under 72 characters.
- Use present tense.
- Prefer atomic commits.
- Prioritize changelog readability.
- Explain why the chosen commit type was selected.
- Base conclusions on actual code changes, not file names alone.
- Always inspect the actual diff content before generating a commit message.
- Never determine the commit type based only on filenames or directory names.
- Prioritize semantic analysis of code changes over file metadata.
- When measurable performance improvements are detected, prefer "perf" over "refactor".
- Use "refactor" only when the primary goal is code restructuring without behavioral or performance impact.

---

## Special Handling

### ADVPL / TLPP

Automatically detect TOTVS Protheus modules and prefer scopes such as:

- sigatms
- sigafat
- sigacom
- sigaest
- sigafat
- protheus

When ADVPL/TLPP code is modified:

- Consider business-rule changes separately from technical refactoring.
- Detect performance improvements.
- Detect SQL optimization.
- Detect framework modernization.

### Frontend

Frameworks:

- React
- Next.js
- TypeScript
- JavaScript

Prefer scopes such as:

- frontend
- ui
- pages
- components
- hooks

### Backend

Technologies:

- Node.js
- PHP
- Python

Prefer scopes such as:

- api
- backend
- auth
- database
- services

### Infrastructure

Technologies:

- Docker
- AWS
- GitHub Actions
- GitLab CI
- Kubernetes

Prefer scopes such as:

- infra
- docker
- aws
- ci
- build

---

## Examples

### Example 1

Input:

```text
/commit
```

Detected:

```text
- Added MFA authentication
- Updated login API
- Added TOTP validation
```

Output:

```text
Resumo das alterações:
Implementação de autenticação multifator no fluxo de login.

Tipo escolhido:
feat(auth)

Motivo:
Foi adicionada uma nova funcionalidade ao sistema.

Commit sugerido:

feat(auth): adiciona autenticação multifator

Corpo opcional:

- Implementa validação TOTP
- Atualiza endpoint de login
- Adiciona testes de autenticação

Comando:

git commit -m "feat(auth): adiciona autenticação multifator"
```

---

### Example 2

Input:

```text
/commit
```

Detected:

```text
- Refactored ADVPL validation routines
- No business rules changed
```

Output:

```text
refactor(sigatms): simplifica validações de cadastro
```

---

### Example 3

Input:

```text
/commit
```

Detected:

```text
- Reduced database queries
- Added caching
```

Output:

```text
perf(api): reduz consultas redundantes ao banco
```

---

## Expected Result

Whenever the user executes:

```text
/commit
```

The skill should:

1. Analyze repository changes.
2. Determine the best Conventional Commit type.
3. Determine the most appropriate scope.
4. Explain the decision.
5. Generate a production-quality commit message.
6. Suggest commit splitting when necessary.
7. Return a ready-to-execute git commit command.

---

## Execution Mode

When invoked using:

/commit

The skill should:

1. Analyze repository changes.
2. Generate the best commit message.
3. Display the proposed commit.
4. Ask for confirmation before executing any Git command.

Example:

Commit sugerido:

feat(sigatms): otimiza validação de romaneio

Executar este commit?

[y] Sim
[n] Não

If the user confirms, execute:

git commit -m "<generated message>"