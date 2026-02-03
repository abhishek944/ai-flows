# PLAN MODE - Comprehensive Feature Planning System

## Placeholders

```
ADD_YOUR_REPO = 'bloom'
ADD_YOUR_REPO-backend = 'bloom-backend'
ADD_YOUR_REPO-ui = 'bloom-ui'
ADD_YOUR_REPO-admin = 'bloom-admin'
```

Below, `<ADD_YOUR_REPO>-*` refers to these codebase folders (read-only in plan mode).

## Overview
Planning mode for AI coding assistants: thorough analysis, clarification, and documentation before implementation. **Read-only** for the codebase (`<ADD_YOUR_REPO>-*` folders); **write** only to `ai-flows/<feature_name>/` for planning artifacts.

## Core Principles

1. **Deep File Analysis** — Read and understand every codebase file related to the feature (structure, patterns, dependencies, conventions). Understand interactions, data flow, and boundaries.

2. **Strict Read-Only for Codebase** — Do not create or modify any file in `<ADD_YOUR_REPO>-backend/`, `<ADD_YOUR_REPO>-ui/`, or `<ADD_YOUR_REPO>-admin/`. Only read from these folders.

3. **Write Access for Planning** — Create and update only in `ai-flows/<feature_name>/`: `plan.md`, `plan-questions.md`, `review-plan.md`, `todo.md`. Create the feature directory if needed.

4. **Exhaustive Questioning** — Ask comprehensive questions before planning. No assumptions. **Use multiple choice for every question**: each question MUST list options (e.g. a), b), c), d) Other — please specify). Prefer multiple choice; use open-ended "User Reply: __" only when options truly cannot be listed. **User answers only** — agent MUST NOT fill "User Reply" sections. Question caps: Easy 5, Medium 15, Big 25. Document in `plan-questions.md` with "User Reply: __".

5. **Feature Naming** — **Auto-set** the feature name from the user's request. Do not ask the user for a feature name. Infer a short, descriptive name (lowercase or camelCase; no spaces; underscores/hyphens allowed). Put `FEATURE: <feature_name>` at top of plan. Example: user says "plan login" → `userLogin` or `login`.

## File Organization

```
ai-flows/<feature_name>/
  plan.md              # Implementation plan (phases if needed)
  plan-questions.md    # Q&A with "User Reply: __"
  review-plan.md       # Review comments
  todo.md              # Implementation todos from plan
```

## Content Constraints

- **No code snippets** in plan.md or todo.md — intent and behavior only.
- **Plan length**: Easy ≤100 lines, Medium ≤200, Big ≤300.
- **Scope**: Exclude testing, deployment, documentation updates, and future enhancements from plan, questions, and todos.

## Invocation Scenarios

Determine which applies and follow its workflow.

### Scenario A: New Plan
**When:** No `ai-flows/<feature_name>/` or no plan.md; user asks for a new plan.

1. **Set feature name** — Infer from user request; do not ask. Use lowercase or camelCase, no spaces; underscores/hyphens allowed. Create `ai-flows/<feature_name>/` using this name.
2. **Initial discovery** — Read only from `<ADD_YOUR_REPO>-*`; understand structure, stack, affected areas. Do not modify codebase.
3. **Questions** — Generate questions (with caps 5/15/25). **Each question MUST include multiple choice options** (e.g. a), b), c), d) Other — please specify). Add "User Reply: __" after options. Write to `ai-flows/<feature_name>/plan-questions.md`. Exclude testing/deployment/docs/future. Present and wait for user.
4. **Plan** (after user answers) — Read "User Reply" from plan-questions.md. Create `ai-flows/<feature_name>/` if needed. Write `plan.md` (FEATURE header, within line cap, no code). If large, use phases. Write `todo.md` from plan. Exclude testing/deployment/docs/future from plan and todos.

### Scenario B: Questions Answered
**When:** plan-questions.md exists and user has filled "User Reply" sections.

1. **Read** — plan-questions.md and any existing plan.md.
2. **Process answers** — Use only user-provided "User Reply" content; do not fill answers yourself.
3. **Follow-ups** — If unclear, add questions to plan-questions.md (within caps); **each with multiple choice options**; wait if needed.
4. **Plan** — Create or update plan.md from answers; respect line cap and no-code rule. Update todo.md if plan structure changed.

### Scenario C: Review Comments Provided
**When:** review-plan.md exists with user feedback.

1. **Read** — review-plan.md and plan.md.
2. **Questions** — If review raises ambiguities, add to plan-questions.md (within caps); **each new question with multiple choice options**; ask user to fill "User Reply" if needed.
3. **Update** — Modify plan.md per review; update todo.md if steps changed. Stay within line cap and no-code rule.

## Plan Structure (plan.md)

Include at minimum:

1. **Feature header** — `FEATURE: <feature_name>`
2. **Executive summary** — Overview, goals, success criteria
3. **Requirements** — Functional/non-functional, user stories, acceptance criteria
4. **Technical analysis** — Relevant architecture, affected components, dependencies, data/API/DB
5. **Implementation strategy** — Approach, tech choices, design patterns
6. **Phases** (if large) — Phase name, objectives, scope, deliverables, dependencies; steps per phase
7. **Detailed steps** — Per phase or overall: description, files to create/modify, changes, dependencies
8. **File-level breakdown** — Path, purpose, key functions/components, imports
9. **Integration points** — Existing code, APIs, DB, external services
10. **Edge cases & errors** — Scenarios, validation, handling
11. **Risks & mitigations**
12. **Open questions** (if any)

No code snippets; describe intent and behavior only.

## Question File (plan-questions.md)

- Categories (e.g. Requirement, Technical, Design). **Every question MUST have multiple choice options** — e.g. a) Option 1, b) Option 2, c) Option 3, d) Other — please specify. Then **User Reply: __**. Do not ask open-ended textual questions; use multiple choice so the user can pick an option. Agent must not fill replies. Stay within 5/15/25 caps. Exclude testing/deployment/docs/future.

**Required format per question:**
```
1. [Question text]
   - a) [Option 1]
   - b) [Option 2]
   - c) [Option 3]
   - d) Other — please specify
   - User Reply: __
```

## Review File (review-plan.md)

Sections: General Feedback; Section-Specific Feedback; Action Items (checkboxes).

## Todo File (todo.md)

- Actionable todos from plan; high-level; no code. Structure: by phase or priority; each todo: description, files, priority, dependencies. Exclude testing, deployment, docs, future work. Use `- [ ]` / `- [x]`.

## Execution Protocol

1. **Determine scenario** — Check ai-flows/ and feature dir; choose A, B, or C.
2. **Discovery** — Read only from `<ADD_YOUR_REPO>-*`; map patterns and dependencies. Do not modify.
3. **Questions** — Generate within caps; **each question with multiple choice options (a, b, c, …)**; then "User Reply: __"; write to plan-questions.md. No self-answers.
4. **Plan** — Create/update plan.md (line cap, no code); extract todo.md.
5. **Present** — Show plan/questions; wait for user input.

## Quality Checklist

- Feature name at top; relevant files analyzed; **every question has multiple choice options**; questions within caps; plan detailed and within line cap; no code in plan/todos; todos in todo.md; scope excludes testing/deployment/docs/future.

## Important Notes

- Do not skip questions; confirm understanding. Read-only for codebase; write only to ai-flows/. Document all Q&A and decisions in ai-flows/. Honor question caps, line limits, no-code rule, and scope exclusions.
