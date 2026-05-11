# AIEP Token

**Same output. Less compute.**

A side-by-side demonstration comparing standard LLM token usage against the AIEP incremental reasoning substrate — with live efficiency metrics.

[![AIEP](https://img.shields.io/badge/AIEP-protocol-0969da)](https://aiep.dev)
[![Licence](https://img.shields.io/badge/licence-Apache%202.0-blue)](./LICENSE)

---

## What it shows

| Column | What it represents |
|--------|-------------------|
| **Standard** | Stateless re-derivation — how many tokens a standard LLM call would consume starting from scratch |
| **AIEP** | Incremental recall substrate — actual tokens used after pulling from committed prior reasoning |

The efficiency panel (expandable) shows the full P117 parametric unburdening breakdown: reused steps, fresh steps, tokens avoided, **HAIR chain recalls**, source diversity score, and the raw evidence artefacts the substrate drew from. When the HAIR cross-session recall overlay fires, the efficiency summary line appends the count of recalled artefacts.

The substrate also exposes a **hash verification** endpoint — any response hash can be checked against the committed ledger without authentication.

---

## Quick start

1. Open `index.html` in any browser — no build step required.
2. Set your PIEA API endpoint on `<body data-api-base="https://your-piea.workers.dev">`.  
   Without this, it defaults to `http://localhost:8788` (local development).
3. Enter a question and click **Run**.

### Running a local PIEA endpoint

If you have a PIEA worker running locally on port 8788, the demo works with no configuration. The demo calls:

- `POST /api/ask` — `{ question, session_id }` → `{ answer, efficiency, evidence_rail, usage, llm_usage, source_diversity_score, connection_rail }`
- `GET /api/stats` — `{ tokens_saved_total, … }`
- `POST /api/recall/verify` — `{ hash: "<64-char SHA-256>" }` → `{ verified, ledger_record }` (unauthenticated)

### `efficiency` object fields

```json
{
  "reuse_pct": 68,
  "reused_steps": 4,
  "fresh_steps": 2,
  "tokens_avoided": 3200,
  "baseline_tokens": 4700,
  "reduction_ratio": 0.68,
  "hair_recalls": 4
}
```

`hair_recalls` is the count of prior artefacts injected by the HAIR cross-session recall overlay. When `hair_recalls > 0`, `tokens_avoided = hair_recalls × 800`; otherwise `tokens_avoided = reused_steps × 500`.

---

## Files

```
index.html    — self-contained demo, no dependencies
README.md     — this file
LICENSE       — Apache 2.0
```

---

## Part of the AIEP ecosystem

- [aiep.dev](https://aiep.dev) — project home
- [AIEP Live](https://github.com/Phatfella/AIEP-LIVE) — real-time operating metrics dashboard
- [AIEP Utilities](https://github.com/Phatfella/AIEP-UTILITIES) — shared tooling for the AIEP protocol

---

## Licence

Apache 2.0 — see [LICENSE](./LICENSE).
