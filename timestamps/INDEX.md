# Timestamp proofs

One OpenTimestamps proof per revision of `README.md`. **No proof is ever deleted.** When the README
changes, the proof in force is renamed to the commit it stamps and a fresh `README.md.ots` is made
for the new text. A proof only verifies against the exact bytes it was made from, so an archived
proof must be checked against that revision pulled out of git history.

| Proof file | Stamps README as of | Covers |
|---|---|---|
| `README.md.df85e73.ots` | commit `df85e73` (2026-09-19) | Genesis rules + corrected verification section. No disclaimer v1. |
| `README.md.ots` | current `HEAD` (2026-09-20) | Same, plus standing disclaimer v1 under `## Disclaimer`. |

Verify the current one:

```
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
