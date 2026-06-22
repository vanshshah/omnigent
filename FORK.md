# Fork notes

This is a **customized fork** of [omnigent-ai/omnigent](https://github.com/omnigent-ai/omnigent),
maintained by [Vansh Shah](https://github.com/vanshshah). Upstream is the real
project; this file is the source of truth for everything *this fork* changes on
top of it, and is fork-owned — keep it across upstream syncs.

Based on upstream **`b5d6a9d`** (`docs(readme): list cursor-native and pi-native harnesses…`, 2026-06-22).

## What this fork adds

Everything here is **additive** — new paths under `examples/`, one new
top-level file, and a single bounded block in `README.md`. No upstream source,
policy, or build path is modified, so `git merge upstream/main` stays clean.

| Addition | Path | What it is |
| --- | --- | --- |
| **`skyline` agent** | [`examples/skyline/`](examples/skyline/) | A careful local data analyst over CSV / parquet / JSON / sqlite — looks before it speaks, reports concrete numbers, never invents a value. Read-only cwd, no network. |
| **`skyline-guard` policy** | [`examples/skyline/guardrails.yaml`](examples/skyline/guardrails.yaml) | A reusable house-guardrails bundle (registered builtins only): ASK before file/shell ops, hard-cap tool calls, downgrade off expensive models past a spend threshold. Also wired inline into the agent. |
| **These fork notes** | [`FORK.md`](FORK.md) + a `README.md` callout | Makes the fork legible on the GitHub homepage and records what diverges from upstream. |

Run the agent:

```bash
omnigent run examples/skyline/ -p "how many rows in data.csv, and what date range does it cover?"
```

## Why

A meta-harness is only as useful as the agents you actually point it at.
`skyline` is the one I reach for over a directory of data, and `skyline-guard`
encodes a personal risk posture — approve before side effects, no runaway
loops, no silent spend on an expensive model — that I want riding along with it.
Both are deliberately built from documented, in-tree primitives so they survive
upstream churn.

## Tracking upstream

This fork tracks `omnigent-ai/omnigent`. To pull in upstream changes:

```bash
# one-time: register the upstream remote
git remote add upstream https://github.com/omnigent-ai/omnigent.git

# then, to sync
git fetch upstream
git merge upstream/main      # or: git rebase upstream/main
```

Because every fork addition lives on a path upstream doesn't touch (plus one
isolated `README.md` block), conflicts should be rare and, if any, confined to
that block — resolve once and keep it.
