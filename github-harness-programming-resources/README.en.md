# GitHub Harness Programming Starter Kit

This repo is a copy-and-use starter kit for running AI work through GitHub.

It packages a small version of Kun's GitHub Harness workflow: project instructions, reusable Skills, GitHub templates, and operating checklists. The goal is practical: copy these files into your own repo, let your AI agent read them, and run work through Discussion, issue, PR, and evidence comments instead of scattered chat.

## Quick Start

### 1. Copy the kit into your repo

Copy these folders and files:

| Source | Put it in your repo | Why |
|---|---|---|
| [`prompts/AGENTS.example.md`](prompts/AGENTS.example.md) | `AGENTS.md` | Project-level instructions for your AI agent |
| [`skills/github-harness-workflow/SKILL.md`](skills/github-harness-workflow/SKILL.md) | `.agents/skills/github-harness-workflow/SKILL.md` | The reusable workflow Skill |
| [`skills/github-cognitive-surface-lite/SKILL.md`](skills/github-cognitive-surface-lite/SKILL.md) | `.agents/skills/github-cognitive-surface-lite/SKILL.md` | The issue/comment writing Skill |
| [`templates/`](templates/) | `.github/` or your docs folder | Discussion, issue, PR, and evidence templates |
| [`checklists/adoption-checklist.md`](checklists/adoption-checklist.md) | `docs/github-harness-checklist.md` | Setup checklist |

You do not need to copy every file on day one. Start with `AGENTS.md`, the workflow Skill, and the three templates: demand Discussion, task issue, evidence comment.

### 2. Ask your AI agent to read the instructions

Use this prompt:

```text
Read AGENTS.md and .agents/skills/github-harness-workflow/SKILL.md.
Then help me run this repo through the GitHub Harness workflow.
Do not start implementation until we have a demand Discussion or a task issue.
```

### 3. Open the first demand Discussion

Use [`templates/discussion-demand-confirmation.md`](templates/discussion-demand-confirmation.md).

The Discussion is where requirements get clarified. It should end with an AI-written demand confirmation commit.

### 4. Split one task issue

Use [`templates/task-issue.md`](templates/task-issue.md).

The issue should be narrow enough that an AI agent can execute it, produce evidence, and stop for review.

### 5. Require an evidence comment

Use [`templates/evidence-comment.md`](templates/evidence-comment.md).

The agent must say what changed, where the evidence is, what is not done, and what should happen next.

### 6. Inner-loop automation + labels

Copy `.github/workflows/` (three workflows) so the loop runs itself:

- `issue-opened-hint.yml` — posts a branch-naming hint on new issues; truth-source issues get a "frozen, do not claim" note.
- `pr-merged-close-issue.yml` — auto-closes `Closes #` issues when a PR merges to `main`; guards `truth-source`; reads Chinese PR bodies; keeps the branch.
- `pr-issue-link-guard.yml` — soft-reminds if a PR body lacks `Closes/Refs` (non-blocking).

Create the label set (`prd` / `truth-source` / `parent-task` / `sub-task` / `task` / `phase-a` / `demo` / `frozen`) — see [`docs/labels.md`](docs/labels.md).

> 💬 **Enable Discussions** in repo Settings → Features, then create a category (e.g. "Demand") in the Discussions UI — the API cannot create categories.

## Living Loop

This repo runs its own kit — it is not an empty file pack. See the real loop:

- [#1 PRD truth-source](https://github.com/kun-content-lab/github-harness-programming-resources/issues/1) — frozen product definition
- [#2 Parent Epic](https://github.com/kun-content-lab/github-harness-programming-resources/issues/2) — 5 native sub-issues, progress auto-sums
- [SI-4 demo PR #9](https://github.com/kun-content-lab/github-harness-programming-resources/pull/9) — `feat/6-demo-loop` → `Closes #6` → merge main → auto-close
- End-to-end walkthrough: [`examples/living-loop-walkthrough.md`](examples/living-loop-walkthrough.md)

## Repository Map

| Path | What it gives you |
|---|---|
| [`docs/how-it-works.md`](docs/how-it-works.md) | The GitHub Harness model |
| [`docs/adoption-guide.md`](docs/adoption-guide.md) | How to install this kit in another repo |
| [`docs/surface-map.md`](docs/surface-map.md) | What Discussion, issue, PR, comment, and board each do |
| [`prompts/`](prompts/) | Public project instruction examples |
| [`skills/`](skills/) | Copyable Skill files for AI agents |
| [`workflows/`](workflows/) | Step-by-step operating loops |
| [`templates/`](templates/) | Demand, task, PR, evidence, and review templates |
| [`checklists/`](checklists/) | Adoption and boundary checks |
| [`examples/`](examples/) | A small example project using the workflow |

## Boundary

This is a public starter kit. It contains reusable workflow instructions, templates, and examples. It does not contain private project material, credentials, personal workspace paths, or account-specific automation.

## License

MIT
