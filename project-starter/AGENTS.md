# AGENTS.md

This repo uses GitHub Harness workflow.

## Goal

Use GitHub as the control plane for AI work. Do not keep long project state only in chat.

## Core Rule

Before implementation, make sure there is one of these:

- a demand Discussion with a demand confirmation commit;
- a task issue with goal, scope, acceptance, and evidence requirements.

If neither exists, stop and create or request one.

## Workflow

1. Read the relevant Discussion, issue, PR, and previous comments.
2. Restate the current goal, boundary, acceptance, and evidence target.
3. Execute only inside the stated scope.
4. If scope changes, return to Discussion or split a new issue.
5. When finished, write an evidence comment.
6. Do not claim completion without evidence.

## Evidence Comment Requirements

Every completion comment must include:

- what changed;
- where to review it;
- what was not done;
- risks or open questions;
- recommended next step: close, continue, split, or return to Discussion.

## Human Review

The human reviews four things:

- direction;
- boundary;
- evidence;
- next step.

## Public / Private Boundary

Do not expose credentials, private paths, private project material, or account-specific settings.

## Skills

Use these local skills when available:

- `github-harness-workflow`: run the demand -> task -> evidence loop.
- `github-cognitive-surface-lite`: write readable GitHub issues, PR bodies, and comments.
