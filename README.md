# Pick Log

A public, append-only record of every call this desk publishes.

Research on AI and technology companies, written by a software engineer who builds AI systems for a living. The premise is simple: **anyone can claim a record after the fact, so this one is written before the fact and never edited.**

---

## The four rules

1. **Logged before posted.** The entry is committed here before the call is published anywhere. If it was not logged first, it never happened.
2. **Nothing is deleted.** Losers stay on the record, marked, permanently. Closed calls move to the closed table — they do not disappear.
3. **Scored against QQQ** from the same timestamp as the entry. "Up 12%" means nothing if QQQ was up 15% over the same window.
4. **Every public post carries a disclaimer and discloses any position held.**

---

## How to verify this record yourself

Every commit in this repository is created through the GitHub API and is **GPG-signed by GitHub's own key** — look for the `Verified` badge on any commit. The commit date is set by GitHub's servers, not by the author's machine.

That means the author **cannot backdate an entry**, cannot quietly edit a thesis after the outcome is known, and cannot delete a losing call without leaving the deletion itself in the history.

```
git clone https://github.com/shikabotai/pick-log
cd pick-log
git log --show-signature
```

If an entry's content ever changes, `git log -p README.md` shows exactly what changed and when.

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

## Scorecard

Published monthly. Every open and closed call, total return vs QQQ over the identical window, hit rate including losers.

No scorecard yet — the first one is published after the first full month of entries.

---

## What is not in this repository

Personal holdings and private research are **not** published here and never will be. Research on a position already owned is not a call, and mixing the two would corrupt this record before it starts. Only forward-looking, publicly-posted calls appear in this file.

---

## Price fields

An entry may open with `PENDING-BACKFILL` in the price column as long as **timestamp, ticker, direction, thesis and exit rule are complete**. Those are the fields that prove the call; the price is arithmetic added afterwards.

Every backfilled price is written as `<price> (backfilled YYYY-MM-DD, source: <source>)`. A price appearing without a backfill stamp marks the entry disputed.

Backfilling a price is the **only** retroactive edit this file permits. Thesis, direction, exit rule and timestamp are frozen at entry, and the git history proves it.

---

## Disclaimer

Nothing in this repository is investment advice, a recommendation, or a solicitation to buy or sell any security. It is a personal research record published for accountability. The author is not a registered investment adviser or broker-dealer. Any position held is disclosed in the entry. Past performance does not predict future results. You are responsible for your own decisions.
