# The log

The entries themselves. The rules that govern them, the verification instructions and the standing
disclaimer live in [`README.md`](README.md) — this file holds nothing but the record.

This file has **its own OpenTimestamps proof chain**, separate from the README's. That is deliberate:
the README changes editorially and rarely, this file changes evidentially and often, and a permitted
price backfill here must never be the reason a README proof breaks. See `timestamps/INDEX.md`.

```
ots verify timestamps/log.md.ots -f log.md
```

---

## Open calls

| # | Date/time (ET) | Ticker | Direction | Entry price | Price source | QQQ at entry | Thesis | Exit rule | Position held | Post |
|---|---|---|---|---|---|---|---|---|---|---|
| — | — | — | — | — | — | — | — | — | — | — |

*No entries yet. This file exists before the first call on purpose — the empty state is part of the proof.*

---

## Closed calls

| # | Ticker | Opened | Closed | Reason closed | Return | QQQ same window | Result |
|---|---|---|---|---|---|---|---|
| — | — | — | — | — | — | — | — |

*Expected to fill with losers. They stay.*

---

## Price fields

An entry may open with `PENDING-BACKFILL` in the price column as long as **timestamp, ticker, direction, thesis and exit rule are complete**. Those are the fields that prove the call; the price is arithmetic added afterwards.

Every backfilled price is written as `<price> (backfilled YYYY-MM-DD, source: <source>)`. A price appearing without a backfill stamp marks the entry disputed.

Backfilling a price is the **only** retroactive edit this file permits. Thesis, direction, exit rule and timestamp are frozen at entry, and the git history proves it.

A backfill is an edit like any other: the proof in force is rotated and this file is re-stamped in the
**same commit**. A commit that changes this file without a matching `timestamps/` change marks that
entry disputed.
