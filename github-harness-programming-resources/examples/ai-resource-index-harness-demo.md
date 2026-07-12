# Example: AI Resource Index Harness

This example shows how to use the starter kit for a small non-code project.

## Demand Discussion

Weak request:

> I want to make a useful AI resource site.

Harness-ready demand:

> This site serves people who want to start learning AI workflows. Version one includes 20 high-quality resources. Each item has category, summary, source, and recommended difficulty. Version one does not include login, community, ranking, or recommendation algorithms.

## Demand Confirmation Commit

```markdown
## Demand confirmation commit

- Target user: people who want to start learning AI workflows.
- First version goal: publish a lightweight index of 20 high-quality resources.
- In scope: category, summary, source link, recommended difficulty, reading order.
- Out of scope: login, community, ranking, recommendation algorithm, auto-crawling.
- Acceptance standard: 20 resources are visible; every item has a source link and short summary; users can browse by category.
- Open questions: final category names.
- Suggested issues: resource list, information structure, homepage draft, evidence screenshot.
```

## Task Issue

```markdown
## Goal

Create the first 20-item resource list.

## Scope

### In Scope

- Select 20 resources.
- Add category, summary, source link, reason, and difficulty.

### Out Of Scope

- No page design.
- No login.
- No ranking algorithm.
- No auto-crawling.

## Acceptance Criteria

- [ ] Exactly 20 items.
- [ ] Every item has a source link.
- [ ] Every item has a short summary.
- [ ] Every item has category and difficulty.
- [ ] Evidence comment is posted.
```

## Evidence Comment

```markdown
## Status

ready-for-review

## What Changed

- Created a 20-item AI workflow resource list.
- Added category, summary, source, reason, and difficulty for every item.

## Evidence

| Evidence | Link or path | What it proves |
|---|---|---|
| Resource list | docs/resources.md | 20 complete items |
| Category section | docs/resources.md#categories | Items grouped by category |

## Not Done

- No homepage design.
- No ranking.
- No auto-update.

## Recommended Next Step

Close this issue and split a new issue for homepage information structure.
```
