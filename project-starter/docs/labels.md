# Label Set

This kit uses a small label set to mark issue layers and lifecycle. Copy them into your repo (Settings → Labels), or via `gh label create`.

| Label | Color | Purpose |
|---|---|---|
| `prd` | `#5319e7` | PRD main line / product demand entry |
| `truth-source` | `#0e8a16` | Frozen standing brief; never enters the claim→close loop |
| `frozen` | `#d93f0b` | Frozen standing item (paired with `truth-source`) |
| `parent-task` | `#1d76db` | Parent Epic — control surface, `Refs` only |
| `sub-task` | `#c5def5` | Sub-task under a parent Epic — `Closes` on completion |
| `task` | `#FBCA04` | One executable unit |
| `phase-a` | `#fbca04` | First-principle MVP slice |
| `demo` | `#a2eeef` | Frozen example for readers, not real development |

GitHub default labels (`bug`, `enhancement`, `documentation`, `good first issue`, `help wanted`, `question`, `wontfix`, `duplicate`, `invalid`) are kept.

## Why no `delegate:*` / `review:*` model labels?

relay-station (the reference impl) labels the executing/reviewing model per issue. This starter kit does **not** — it is model-agnostic. Configure your own `delegate:`/`review:` labels if you multi-model; they are optional here.

## Create via CLI

```bash
REPO=<owner>/<repo>
gh label create prd --color 5319e7 --description "PRD main line" --repo $REPO
gh label create truth-source --color 0e8a16 --description "Frozen standing brief" --repo $REPO
gh label create parent-task --color 1d76db --description "Parent Epic" --repo $REPO
gh label create sub-task --color c5def5 --description "Sub-task" --repo $REPO
gh label create task --color FBCA04 --description "Executable unit" --repo $REPO
gh label create phase-a --color fbca04 --description "First MVP" --repo $REPO
gh label create demo --color a2eeef --description "Frozen example" --repo $REPO
gh label create frozen --color d93f0b --description "Frozen" --repo $REPO
```