# AGENTS.md

This file defines how to use Codex as the primary engineering agent in this repo.

## Primary Operating Mode

- Use Codex for end-to-end work: plan, implement, test, open PR, review, and follow-up fixes.
- Prefer one active Codex thread per task to preserve context and reduce repeated setup.
- Always run a verification pass before opening or updating a PR.

## MCP Profile (Codex-Only)

### Required MCPs

- `filesystem`: read/edit code safely and quickly
- `github`: create branches, commits, PRs, PR comments/reviews

### Recommended MCPs

- `memory`: keep persistent project decisions, conventions, and TODO context
- `context7`: fetch current docs when API/framework behavior is uncertain

### Optional MCPs

- `playwright`: E2E checks and UI regression verification
- `desktop-commander`: local automation when shell-only flow is not enough

## Prompt Patterns

### 1) Feature Delivery

Use this prompt:

```txt
Implement <feature> in this repo.
Constraints:
- Do not break existing behavior.
- Add/update tests.
- Run lint/typecheck/tests.
- Commit in logical steps.
- Open/update PR with: summary, changes, test evidence, risks, rollback plan.
If blocked, state exact blocker and propose smallest safe workaround.
```

### 2) Bug Fix

Use this prompt:

```txt
Investigate and fix <bug>.
First reproduce it, then identify root cause, then patch minimally.
Add regression test(s), run validation, and update PR description with:
- root cause
- fix
- verification
- side effects checked
```

### 3) PR Review

Use this prompt:

```txt
Review PR #<number> with a bug-risk focus.
Prioritize: correctness, regressions, missing tests, security, perf, edge cases.
Return:
1) findings by severity with file references
2) open questions/assumptions
3) concise summary
```

## Definition of Done (DoD)

- Code compiles and passes repo checks
- Tests updated for changed behavior
- PR description includes what/why/how tested
- Risks and rollback noted for non-trivial changes

## Repo-Specific Path Rules

### `apps/web`

- Prioritize UX-safe changes and avoid breaking public routes.
- Include component-level or integration test updates for behavior changes.

### `apps/docs`

- Keep docs aligned with actual shipped behavior and commands.
- Include usage examples when introducing new developer workflows.

### `packages/ui`

- Preserve API stability for shared components.
- Highlight any prop/interface changes in PR notes.

### `packages/env`

- Treat as sensitive configuration surface.
- Document new env vars and defaults; avoid silent behavior changes.

### `packages/eslint-config` and `packages/tsconfig`

- Treat as repo-wide blast-radius changes.
- Mention migration impact and verify all workspace apps/packages.

## Working Agreements

- Do not introduce broad refactors unless requested.
- Prefer small, reviewable commits.
- If unexpected unrelated changes are found, pause and ask before proceeding.

