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

### The benchmark leg — `QQQ at entry` (added 2026-09-20)

Rule 3 in [`README.md`](README.md) scores every call against QQQ. That column is subject to everything
above, plus four things that only apply to it.

**Both prices are the close of the entry date, and that is a correction to Rule 3's wording.** Rule 3
says "from the same timestamp as the entry". The source used here publishes **daily closes only** —
there is no intraday in it. Pairing an intraday entry price against a closing benchmark would insert
half a session of drift into every published spread, invisibly. So both legs are the entry date's
close: an entry logged before or during the session opens with `PENDING-BACKFILL` in **both** price
cells and is filled after that close; an entry logged after the close can open complete; an entry
dated a non-trading day uses the prior trading day's close and stamps *that* date.

**Both legs are pulled in the same session and filled in the same commit.** Never one now and the
other later. The source restates historical prices after corporate actions such as splits — a close
from before a split reads divided by the split factor today, with nothing in the response saying so —
so two legs looked up on two different dates are not a comparable pair, and the row would look fine.

**Deadline: the close of the next trading day after the entry date.** A row still carrying
`PENDING-BACKFILL` in either price cell after that is marked **`UNBENCHMARKED`** in the row, and that
mark is **permanent — it is not removed if the price is filled in afterwards**. Unbenchmarked rows
stay in the table above, stay out of every published return and spread, and are counted on a standing
*"calls we cannot score: N"* line in the monthly scorecard that prints even when N is zero.

**This deadline is a completeness rule, not an anti-fraud rule.** A historical close is a fixed fact,
so a late backfill cannot be cherry-picked and we do not claim it could be. What a missing benchmark
cell does is make the row unscoreable against the only thing this record measures — and a record that
quietly drops the rows it failed to finish is the exact failure this file exists to prevent.
