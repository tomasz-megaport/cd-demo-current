# cd-demo-current

> "What we do today" — fully manual CD pipeline. Every release is a human in the loop.

This repo demonstrates the **current** state of `web-monorepo` continuous delivery: nothing auto-deploys, every release is a sequence of clicks, every rollback is a fresh release.

Open the GitHub Pages site for the interactive walkthrough — Mermaid diagram + step-by-step `[Next]`/`[Prev]` UI calls out where humans get stuck and the friction it causes.

## Pain points (callouts in the walkthrough)

- **Slack queue** — `#sw-eng-release` is the release record; no tooling, no state
- **Manual click** — engineer navigates Actions → Release → fills form → clicks. 10–20× per day
- **Sequential, not parallel** — staging and prod deploy in the same workflow run with no soak window between them
- **No rollback path** — to undo a bad deploy: revert PR + retag + click again (~20 min)
- **No release dashboard** — to know what's live, scroll Slack
- **No alert correlation** — a deploy can break prod silently for hours

## Pipelines

| Workflow | Trigger | What it does |
|---|---|---|
| `ci.yml` | push / PR to `main` | lint + smoke test |
| `release.yml` | `workflow_dispatch` | bumps tag, deploys to staging then prod sequentially in one run |
| `deploy.yml` | `workflow_dispatch` | redeploy a single env from any ref (used for manual rollback) |

## Local

```bash
pnpm install
pnpm test
pnpm serve     # http://127.0.0.1:4173
```

## Deploy (dry-run)

```bash
deploy/deploy.sh staging --dry-run
```

## Sibling demos

- **MVP target:** [cd-demo-mvp](https://tomasz-megaport.github.io/cd-demo-mvp/) — auto staging + cron-promote prod + 1-click rollback
- **Ideal target:** [cd-demo-ideal](https://tomasz-megaport.github.io/cd-demo-ideal/) — full test→staging→prod gating + flake quarantine + admin dashboard

This demo: <https://tomasz-megaport.github.io/cd-demo-current/>
