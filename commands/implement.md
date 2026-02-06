# IMPLEMENTATION MODE - Self-Sufficient End-to-End Execution

## Configuration

```
bloom-backend
bloom-ui
bloom-admin
```

Below, "codebase folders" refers to these folders (read/write per command rules).

## Overview
Execute feature development autonomously: read planning documents, understand existing implementation, and complete all tasks without user intervention until every todo is done.

## Core Principles

1. **Complete Autonomy** — Do not ask the user any questions. Make all decisions from the plan, plan-questions, and codebase analysis. Resolve ambiguities using existing patterns, conventions, and sensible defaults.

2. **End-to-End Completion** — Complete every todo in `todo.md`. Do not stop with partial implementation. Implement code, tests, and any planned documentation/configuration.

3. **Todo Tracking** — Read `todo.md` before starting. After each completed todo, mark it complete in `todo.md` (`[ ]` → `[x]`). Update continuously; respect dependency order.

4. **Existing Implementation** — Before coding, identify what already exists. Read existing code, ensure alignment with the plan, and build on it (or refactor if it deviates). Do not duplicate or break working code.

5. **Iterative & Continuous** — Continue until all todos are complete. Fix errors immediately, verify as you go, and adapt if you discover issues.

## Runtime Restrictions
- Do not run commands that start the full system or dev servers (e.g. `npm run dev`).
- Limit verification to static analysis, linting, and tests that do not require the full stack.

## Documents to Read Before Implementation

1. **`/ai-flows/<feature_name>/plan.md`** — Feature spec, phases, files to create/modify, integration points, testing and edge-case requirements.
2. **`/ai-flows/<feature_name>/plan-questions.md`** — All Q&A and decisions.
3. **`/ai-flows/<feature_name>/todo.md`** — All todos, dependencies, and priorities.

Then: search and read files mentioned in the plan; understand architecture and patterns; check which todos (if any) are already done.

## Implementation Workflow

### Preparation
- Read plan.md, plan-questions.md, and todo.md completely.
- Analyze codebase: files in plan, structure, conventions, similar features.
- Assess current state: which files exist, what’s done vs remaining; order todos by dependencies.

### For Each Todo (in order)
1. **Read** — Understand the todo, files involved, and dependencies; check if partially done.
2. **Check existing** — If files exist, read them and identify what to add or change.
3. **Implement** — Create/modify files; follow codebase patterns, naming, and imports.
4. **Test** — Add or update tests (happy path, edge cases, errors).
5. **Verify** — Syntax, imports, linters; no breaking changes.
6. **Mark complete** — In `todo.md`: `- [x] Todo description` (optionally add a brief completion note).
7. **Continue** — Next todo; repeat until all are done.

### After All Todos
- Integration: components work together, data flow and API contracts hold.
- Tests: all pass; add missing cases if needed.
- Documentation: update comments/README/API docs as planned.
- Final check: all todos `[x]`, no regressions, no linter errors.

## Todo File Format

```markdown
- [ ] Todo description
- [x] Completed todo description
```

Mark the exact item complete; keep original text; change only `[ ]` to `[x]`. Add a short note only if you deviated from the plan.

## Handling Existing Code

- **Matches plan** — Verify and mark todo complete.
- **Partial** — Read fully, add missing parts, integrate.
- **Different approach** — If it meets requirements, keep and mark complete; otherwise refactor to plan.
- **Incomplete work** — Continue from current state and finish remaining parts.

## Decision-Making (No User Questions)

- **Source of truth** — plan.md and plan-questions.md.
- **Patterns** — Naming, structure, and testing style from the codebase.
- **Stack** — Use existing libraries and frameworks only.
- **Defaults** — When plan is silent: follow project structure, sensible config, clear errors/logging.

## Quality & Errors

- **Code** — Readable, maintainable, consistent with codebase; no known security issues.
- **Tests** — Adequate coverage; no starting dev servers for validation.
- **On error** — Analyze, fix (dependencies/typos/syntax), verify, then continue.
- **If plan unclear** — Use codebase and best practices; make reasonable assumptions and document in comments.

## Execution Protocol

1. **Initialize** — Resolve feature name; confirm `ai-flows/<feature_name>/` and required docs exist; read all planning docs.
2. **Understand** — Feature from plan, requirements from Q&A, todos and dependencies, codebase and existing implementation.
3. **Order** — Sequence todos by dependencies; identify critical path.
4. **Execute** — For each todo: read → check existing → implement → test → verify → mark complete; continue until all done.
5. **Verify & complete** — All todos `[x]`, integration and tests pass, quality check done.

## Important Rules

1. **NO QUESTIONS** — Never ask the user for clarification or decisions.
2. **COMPLETE ALL TODOS** — Do not stop until every todo is done.
3. **MARK TODOS** — Update todo.md after each completion.
4. **UNDERSTAND FIRST** — Read existing code before modifying.
5. **FOLLOW PATTERNS** — Match codebase conventions.
6. **TEST & VERIFY** — Tests and checks as you go; no regressions.

## Example

User: "Implement the user-authentication feature"

1. Read `ai-flows/user-authentication/plan.md`, `plan-questions.md`, `todo.md`.
2. Analyze codebase for auth patterns; check for existing auth code.
3. Complete todos in order: e.g. user model → auth service → login endpoint → tests.
4. Mark each todo `[x]` in todo.md as completed.
5. Run integration and tests; confirm all todos complete and report: "Implementation complete. All N todos finished."
