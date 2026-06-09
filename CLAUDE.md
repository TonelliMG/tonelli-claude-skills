# CLAUDE.md

Global behavioral guidelines. Merge with project-specific instructions; project files win on conflict.

**Tradeoff:** these guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

- State assumptions explicitly. If uncertain, ask.
- Multiple interpretations? Present them — don't pick silently.
- Simpler approach exists? Say so. Push back when warranted.
- Unclear? Stop, name what's confusing, ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility"/"configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Most tasks are simple and need none of the heavy machinery below — just do the work. Ask: "Would a senior engineer call this overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

- When I ask for a change, edit only what that change requires. Don't touch code unrelated to the request or that doesn't affect the flow.
- Don't "improve" adjacent code, comments, or formatting. Don't refactor what isn't broken. Match existing style.
- Notice unrelated dead code? Mention it — don't delete it.
- Remove imports/vars/functions *your* change orphaned; leave pre-existing dead code.

The test: every changed line traces directly to the request.

## 4. Goal-Driven Execution

**Define success criteria. Verify before claiming. Evidence before claims.**

- "Add validation" → cover invalid inputs, then confirm they're handled.
- "Fix the bug" → reproduce it, then confirm the fix.

Never claim done/fixed/passing without running the check and seeing the output.

## 5. Communication & Cost

- Be concise. No filler, no restating the question, no narrating obvious steps.
- Reuse context already established — don't re-derive known facts.
- When acting on a clear request, act; don't survey options you won't pursue.
- When I describe a bug and ask for the solution, give me the solution — I usually debug myself.

## 6. Environment & Stack Conventions

- Windows + PowerShell by default: `$null` not `/dev/null`, `$env:VAR` not `$VAR`, backtick for line continuation.
- **TOTVS Protheus (AdvPL/TLPP):** source files must be **Windows-1252 (CP1252)** — UTF-8 breaks the compiler.
- **React / Next.js:** always follow the existing project's pattern. Some use Tailwind, some shadcn/ui, some hand-written CSS — detect what the project uses and match it. Never introduce a styling approach the project doesn't already use.

## 7. My Skills (optional — use only when needed)

I keep a curated skill set (repo: `tonelli-claude-skills`). **Skills are not always installed** — not every project has them, and even the global ones may be absent. **If a skill isn't installed, just continue the work normally without it — don't announce its absence or try to reinvent it.**

**Use a skill only when the task genuinely calls for it.** Don't reach for one on simple work. Specifically:

- `writing-plans` / `executing-plans` — only when I explicitly say I want to plan something.
- `systematic-debugging` — only when I explicitly ask you to debug. Otherwise I'll explain the problem and just want the fix.
- `requesting-code-review` — worth using to sanity-check my changes before wrapping up.
- `tonelli-git-commit-generator` (`/commit`) / `tonelli-git-merge-request-generator` (`/merge`) — only when I call them.
- `finishing-a-development-branch`, `verification-before-completion` — when the moment fits.

Reference of what exists, grouped by where it lives. **Only load/consider skills in scope for the current project — ignore the rest.**

**Global (`~/.claude/skills`) — workflow:**
`writing-plans`, `executing-plans`, `systematic-debugging`, `verification-before-completion`, `requesting-code-review`, `finishing-a-development-branch`, `tonelli-git-commit-generator`, `tonelli-git-merge-request-generator`.

**Local per project (`.claude/skills`) — by stack:**
- **TOTVS Protheus (AdvPL/TLPP):** `advpl-to-tlpp-migration`, `mvc-generator`, `tlpp-rest-endpoint-generator`, `fwrest-client-generator`, `entry-point-designer`, `query-builder`, `data-dictionary-lookup`, `sql-optimization`, `code-review`, `refactor`, `context-map`, `utf8-to-cp1252-conversion`.
- **React:** `building-components`, `composition-patterns`, `react-best-practices`, `web-design-guidelines`.
- **Next.js:** `building-components`, `composition-patterns`, `react-best-practices`, `web-design-guidelines`, `next-best-practices`.

The personal `tonelli-*` skills communicate in **pt-BR**; code identifiers stay as-is.

---

**Working if:** fewer unnecessary diff lines, fewer rewrites from overcomplication, and clarifying questions come *before* implementation rather than after mistakes.
