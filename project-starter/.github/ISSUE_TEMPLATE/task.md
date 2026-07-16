# Task Issue Template

> 📋 Teaching mirror. Live templates: `task` at `.github/ISSUE_TEMPLATE/task.md`; also see `parent-task.md` / `sub-task.md` / `truth-source.md` for the three issue layers. GitHub uses the `.github/` ones natively; copy either.

## Goal

What should this task complete?

## Source

- Discussion:
- Demand confirmation commit:
- Related issue or PR:

## Scope

### In Scope

-

### Out Of Scope

-

## Inputs

| Input | Location | Why it matters |
|---|---|---|
|  |  |  |

## Deliverables

| Deliverable | Review location |
|---|---|
|  |  |

## Acceptance Criteria

- [ ] The result matches the confirmed demand.
- [ ] The work stays inside this issue scope.
- [ ] Deliverables can be opened, reviewed, or run.
- [ ] Evidence comment is posted before close.

## Agent Instructions

- Restate goal, scope, acceptance, and evidence target before implementation.
- If requirements are unclear, return to the Discussion.
- If scope changes, split a new issue.
- After execution, write an evidence comment.

## Close Condition

Close only when acceptance criteria are met and evidence is reviewable.

## Deletion-spec

What this task added, and how to roll it back or remove it later:
-

## Layer (pick one)

- `task` — one executable unit (default).
- `sub-task` — slice under a parent Epic; PR uses `Closes #<sub>`.
- `parent` Epic — control surface only; PR uses `Refs #<parent>`, never `Closes`.
- `truth-source` — frozen brief; never claimed, never auto-closed.
