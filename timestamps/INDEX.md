# Timestamp proofs

**Two chains, one per stamped file.** `log.md` holds the entries and is re-stamped on every entry and
every permitted price backfill; `README.md` holds the rules and is re-stamped only when the rules
change. Keeping them apart means a broken README proof is always an editorial event, never a side
effect of logging a call.

**No proof is ever deleted.** When a stamped file changes, the proof in force is renamed to the commit
it stamps — `<file>.<sha>.ots` — and a fresh `<file>.ots` is made for the new text. A proof only
verifies against the exact bytes it was made from, so an archived proof must be checked against that
revision pulled out of git history.

## `log.md` — the entries

| Proof file | Stamps `log.md` as of | Covers |
|---|---|---|
| `log.md.ots` | current `HEAD` (2026-09-20) | First stamp of this chain. Open calls and closed calls split out of `README.md`, both tables still empty, plus the price-fields rule. |

## `README.md` — the rules

| Proof file | Stamps `README.md` as of | Covers |
|---|---|---|
| `README.md.df85e73.ots` | commit `df85e73` (2026-09-19) | Genesis rules + corrected verification section. Entry tables still in README. No disclaimer v1. |
| `README.md.3ee2826.ots` | commit `3ee2826` (2026-09-20) | Same, plus standing disclaimer v1 under `## Disclaimer`. Entry tables still in README. |
| `README.md.1aeb6d1.ots` | commit `1aeb6d1` (2026-09-20) | Entry tables and the price-fields rule moved out to `log.md`; README points at it; verification section documents both chains. Three witnesses listed, no re-stamp check. |
| `README.md.ots` | current `HEAD` (2026-09-20) | Adds **witness 4** to the verification section — the reader-runnable check that any commit touching a stamped file also re-stamped it, plus the warning that a `timestamps/INDEX.md` change is not evidence of a re-stamp. |

Verify a current one:

```
ots verify timestamps/log.md.ots -f log.md
ots verify timestamps/README.md.ots -f README.md
```

Verify an archived one:

```
git show df85e73:README.md > /tmp/old-README.md
ots verify timestamps/README.md.df85e73.ots -f /tmp/old-README.md
```

A freshly created proof is **pending** for a few hours: the calendar servers have it, but it is not
yet anchored in a Bitcoin block. `ots upgrade <proof>` pulls the completed attestation down once the
block confirms. Pending is not the same as invalid — it means the anchor has not confirmed yet.
