# Skill Name

tonelli-git-merge-request-generator

## Description

Generates high-quality Merge Request summaries by analyzing the actual differences between two Git branches, including commits, file modifications, and code changes.

This skill is optimized for GitLab workflows and automatically produces a structured MERGE.md document suitable for use as a Merge Request description.

The generated content focuses on providing a clear overview of the delivered changes, impacted areas, business implications, testing considerations, and technical details derived from the actual diff rather than relying solely on commit messages.

The skill should always communicate with the user in Brazilian Portuguese (pt-BR), regardless of the language used internally for analysis.

---

## Trigger

Primary trigger:

```text
/merge <source_branch> <target_branch>
```

Examples:

```text
/merge feature/TMS-123 develop

/merge develop homolog

/merge hotfix/login main
```

---

## Behavior

When activated, the skill must:

### 1. Validate the provided branches

Verify that both branches exist locally or remotely.

If necessary, execute:

```bash
git fetch --all
```

---

### 2. Analyze branch differences using

```bash
git log <target_branch>..<source_branch> --oneline

git diff <target_branch>...<source_branch> --name-status

git diff <target_branch>...<source_branch> --shortstat

git diff <target_branch>...<source_branch>
```

---

### 3. Detect

* Modified files
* Added files
* Deleted files
* Renamed files
* Exclusive commits present in the source branch
* Number of changed files
* Number of inserted lines
* Number of removed lines
* Areas of the system affected by the changes

---

### 4. Identify impacted areas

Infer the impacted areas based on the actual diff content.

Examples:

* Authentication
* API
* Frontend
* Backend
* Database
* Infrastructure
* Docker
* AWS
* CI/CD
* ADVPL
* TLPP
* Protheus
* SIGATMS
* SIGAFAT
* SIGACOM

The analysis must prioritize the semantic meaning of the code changes rather than relying exclusively on filenames or directory structures.

---

### 5. Generate an executive summary

Create a concise and business-friendly explanation describing:

* What was implemented;
* What problems were solved;
* What improvements were introduced;
* Which workflows or modules were impacted.

The summary should be understandable by both technical and non-technical stakeholders.

---

### 6. Detect technical categories

Automatically identify whether the Merge Request includes:

* New features;
* Bug fixes;
* Refactoring activities;
* Performance improvements;
* Infrastructure changes;
* CI/CD updates;
* Test additions or modifications;
* Documentation updates;
* Breaking changes.

---

### 7. Generate testing recommendations

Based on the modified areas, suggest validation scenarios that should be executed before approval.

Examples:

* Functional testing;
* Regression testing;
* API validation;
* User acceptance testing;
* Infrastructure verification.

---

## Output Language

**IMPORTANT**

All user-facing responses MUST be written in Brazilian Portuguese (pt-BR).

The generated MERGE.md content MUST also be written entirely in Brazilian Portuguese.

Never generate Merge Request descriptions in English.

---

## Output Format

The skill must always generate a MERGE.md document following the structure below.

```markdown
# Resumo do Merge Request

## Branch de origem

<source_branch>

## Branch de destino

<target_branch>

## Objetivo

Resumo executivo descrevendo o propósito das alterações.

## Principais alterações

- Alteração identificada 1
- Alteração identificada 2
- Alteração identificada 3

## Áreas impactadas

- Área 1
- Área 2
- Área 3

## Regras de negócio impactadas

Descrever regras de negócio afetadas quando aplicável.

Caso não existam impactos relevantes:

Nenhuma regra de negócio impactada identificada.

## Infraestrutura

Descrever alterações relacionadas à infraestrutura, pipelines ou deploy.

Caso não existam:

Nenhuma alteração de infraestrutura identificada.

## Testes recomendados

- Teste 1
- Teste 2
- Teste 3

## Estatísticas

- X arquivos alterados;
- Y linhas adicionadas;
- Z linhas removidas.

## Commits incluídos

- Commit 1
- Commit 2
- Commit 3

## Observações

Informações adicionais relevantes para revisores e aprovadores.
```

---

## Rules

* Always analyze the actual Git diff before generating the document.
* Never rely exclusively on commit messages.
* Prioritize semantic understanding of the code changes.
* Avoid generic descriptions such as:

  * "Various improvements";
  * "General adjustments";
  * "Bug fixes";
  * "Minor changes";
  * "Several updates".
* Summaries must clearly describe the purpose and impact of the delivery.
* Focus on generating content useful for code reviewers, testers, technical leaders, and business stakeholders.
* The generated document must be concise but sufficiently detailed to support the approval process.
* Do not speculate about functionality that cannot be inferred from the diff.
* When uncertainty exists, explicitly indicate that the conclusion was inferred from the available changes.

---

## Special Handling

### ADVPL / TLPP

When ADVPL or TLPP code is detected:

* Identify impacted Protheus modules;
* Distinguish technical refactoring from business-rule modifications;
* Highlight SQL optimizations;
* Detect performance-related changes;
* Identify framework modernization initiatives.

Preferred impacted areas include:

* SIGATMS
* SIGAFAT
* SIGACOM
* SIGAEST
* PROTHEUS

---

### Frontend

Frameworks:

* React
* Next.js
* TypeScript
* JavaScript

Emphasize impacts related to:

* User experience;
* Components;
* Pages;
* Hooks;
* Client-side validation.

---

### Backend

Technologies:

* Node.js
* PHP
* Python

Emphasize impacts related to:

* APIs;
* Services;
* Authentication;
* Integrations;
* Database interactions.

---

### Infrastructure

Technologies:

* Docker
* AWS
* GitHub Actions
* GitLab CI
* Kubernetes

Highlight impacts involving:

* Deployment pipelines;
* Infrastructure provisioning;
* Containerization;
* Monitoring;
* Automation.

---

## Expected Result

Whenever the user executes:

```text
/merge <source_branch> <target_branch>
```

The skill should:

1. Analyze the differences between the two branches;
2. Identify the commits involved;
3. Inspect the actual code changes;
4. Determine the impacted areas;
5. Generate an executive summary;
6. Produce a complete MERGE.md document;

---

## Execution Mode

When invoked using:

```text
/merge <source_branch> <target_branch>
```

The skill should:

1. Validate the provided branches;
2. Analyze commits and diffs;
3. Generate the MERGE.md content;