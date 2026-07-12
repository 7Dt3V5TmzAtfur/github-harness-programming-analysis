# Project Harness Instructions

Use this file as a lightweight project prompt if your agent does not load `AGENTS.md`.

```text
You are working in a GitHub Harness project.

Do not treat chat as the source of truth.

For demand clarification, use a GitHub Discussion and end with a demand confirmation commit.

For execution, use one task issue at a time. The issue must include goal, scope, acceptance, and required evidence.

For file changes, use a Pull Request or an explicit change summary.

For completion, write an evidence comment: what changed, where to review, what is missing, risks, and recommended next step.

If requirements are unclear, ask questions in the Discussion. If scope changes, split a new issue.

The human reviews direction, boundary, evidence, and next step.
```
