# Adoption Guide

Use this guide when you want to copy this kit into another repo.

## Step 1: Add Project Instructions

Copy:

```text
prompts/AGENTS.example.md -> AGENTS.md
```

Then edit the placeholders:

- project name
- project goal
- owner review rule
- proof surfaces
- public/private boundary

## Step 2: Add Skills

Copy:

```text
skills/github-harness-workflow/SKILL.md
skills/github-cognitive-surface-lite/SKILL.md
```

Suggested target:

```text
.agents/skills/github-harness-workflow/SKILL.md
.agents/skills/github-cognitive-surface-lite/SKILL.md
```

Tell your AI agent:

```text
Use github-harness-workflow when planning or executing work in this repo.
Use github-cognitive-surface-lite when writing issues, PR descriptions, or evidence comments.
```

## Step 3: Add GitHub Templates

Copy the templates you need:

| Template | Use |
|---|---|
| `templates/discussion-demand-confirmation.md` | Demand Discussion |
| `templates/task-issue.md` | Task issue |
| `templates/pr-description.md` | Pull Request |
| `templates/evidence-comment.md` | Completion comment |
| `templates/review-checklist.md` | Human review |

You can also copy `.github/ISSUE_TEMPLATE/task.md` and `.github/PULL_REQUEST_TEMPLATE.md` directly.

## Step 4: Run The First Loop

1. Open a demand Discussion.
2. Ask the agent to ask missing questions.
3. Ask the agent to write a demand confirmation commit.
4. Split one task issue.
5. Let the agent execute.
6. Require an evidence comment.
7. Review direction, boundary, evidence, and next step.

## Step 5: Scale Only When Needed

Add a board only when there are multiple issues.

Add PR review rules only when file changes become frequent.

Add automation only after the manual loop is clear.
