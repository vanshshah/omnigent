# skyline

A careful local data analyst — point it at a directory of data and ask.

skyline reads what's sitting in your working directory (CSV, parquet, JSON,
sqlite) and answers the way you'd want a careful analyst to: it looks before it
speaks — row counts, dtypes, ranges — runs small targeted shell instead of
slurping whole files, reports concrete numbers with units, and never invents a
value it didn't see. When a result reads better as a shape than a table, it
describes the distribution in one line — min — median — max, where the mass
sits, the outliers worth a second look.

The name is a nod to nucleus' "Innings Skyline": data has a silhouette, and
half of analysis is reading it.

## Run it

```bash
# interactive
omnigent run examples/skyline/

# one-shot
omnigent run examples/skyline/ -p "how many rows in data.csv, and what date range does it cover?"
omnigent run examples/skyline/ -p "what's the distribution of the `amount` column — median, spread, outliers?"
```

No model is pinned, so skyline runs on whatever Claude provider you configured
with `omnigent setup` (or `ANTHROPIC_API_KEY`).

## Safety posture

skyline runs with local OS tools but a deliberately tight sandbox:

- **read-only cwd** — it inspects your data, it never mutates it; scratch
  artifacts go to `$TMPDIR`.
- **no network** — this agent reads local data, it does not phone home.
- **platform-default sandbox** — `darwin_seatbelt` on macOS, `linux_bwrap` on
  Linux (the `sandbox.type` is left unset on purpose so one file works on both).

Layered on top of that is a set of house guardrails — see
[`guardrails.yaml`](guardrails.yaml) — that ask before any real-world side
effect, hard-cap tool calls against a runaway loop, and downgrade off expensive
models past a spend threshold. They're declared inline in
[`config.yaml`](config.yaml); `guardrails.yaml` is the standalone, paste-into-
any-agent copy.
