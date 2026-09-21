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

Two independent witnesses, neither of them the author.

**1. GitHub's public event stream.** Every push to this repository emits a public `PushEvent` with a timestamp set by GitHub's servers, not by the author's machine. Those events are permanently archived by the third-party [GH Archive](https://www.gharchive.org/) project and queryable by anyone. An entry cannot be backdated past the push that first published it.

**2. Bitcoin, via OpenTimestamps.** Each revision of this log is hashed and anchored to the Bitcoin blockchain. The proof files live in `timestamps/`. Anyone can check one without trusting GitHub, this author, or anything else:

```
git clone https://github.com/shikabotai/pick-log
cd pick-log
ots verify timestamps/log.md.ots -f log.md        # the entries
ots verify timestamps/README.md.ots -f README.md  # the rules
```

A proof that verifies means that exact text existed at that block height. Rewriting history breaks the proof, visibly.

**Two chains, one per file.** `log.md` holds the entries and is stamped every time an entry or a backfill lands; `README.md` holds the rules and is stamped only when the rules change. A broken README proof is therefore always an editorial event, never a side effect of logging a call.

`timestamps/<file>.ots` always stamps the **current** revision of that file. Every earlier revision keeps its own proof, named for the commit it stamps, and none is ever deleted — see `timestamps/INDEX.md`. To check an old one, pull that revision out of git history and verify against it:

```
git show df85e73:README.md > /tmp/old-README.md
ots verify timestamps/README.md.df85e73.ots -f /tmp/old-README.md
```

**3. The history itself.** `git log -p log.md` shows every change ever made to the entries, including any attempt to alter a thesis after the outcome was known. `git log -p README.md` does the same for the rules.

**4. Every commit that touched a stamped file re-stamped it.** The rule is that a commit changing `log.md` or `README.md` without a matching new proof in the same commit marks that entry **disputed** — not deleted, not quietly fixed, marked. You do not have to take that on trust. Run it on any commit:

```
c=$(git show --pretty= --name-only <sha>)
for f in README.md log.md; do
  echo "$c" | grep -qx "$f" || continue
  echo "$c" | grep -qx "timestamps/$f.ots" || echo "DISPUTED: $f edited without re-stamp"
done
```

Note what this check deliberately does **not** accept as evidence: a change to `timestamps/INDEX.md`. The index is prose, and it is edited on every single entry — so "the commit touched `timestamps/`" proves nothing. Only a new `timestamps/<file>.ots` does.

**5. Reading the result of witness 2 — the message, not the exit code.** Measured on this repository, 2026-09-20.

`ots verify` **exits `1` on a sound record**, and exits `1` on a bad one, and exits `1` when you typed the command wrong. The exit code carries no information at all. The first line of output does. Four outcomes:

| What it prints | What happened | Is the record sound? |
|---|---|---|
| `Could not connect to Bitcoin node: …` | You have no Bitcoin node. The digest already matched; `ots` got as far as checking the block and stopped. | **Yes. On a machine with no node, this is the pass.** |
| `Assuming target filename is …` then `Could not open target: …` | You left out `-f`, so `ots` guessed a filename that does not exist. | Unjudged — nothing was checked. Re-run with `-f`. |
| `File does not match original!` | The file you passed does not hash to the digest in the proof. | **Either the file was altered, or you passed the wrong file.** Settle it below. |
| `Success! Bitcoin block N attests existing data as of …` | Full verification against a node. | Yes, confirmed to a block. |

So a reader without a Bitcoin node — which is nearly every reader — **can never get a success line out of `ots verify`.** The node error is the best answer this command can give you. Reading it as tampering is the mistake this witness exists to prevent.

`-f` is not optional. Both argument orders are identical:

```
ots verify -f log.md timestamps/log.md.ots
ots verify timestamps/log.md.ots -f log.md
```

**`File does not match original!` is not by itself evidence of tampering.** It prints the same line when you simply aim the command at the wrong file — checked here by verifying `README.md` against `log.md`'s proof, which is innocent and indistinguishable. Settle it with the digest, which needs no node and no network:

```
shasum -a 256 log.md                      # 25e4ceb...
ots info timestamps/log.md.ots | head -1  # File sha256 hash: 25e4ceb...
```

If those two strings are equal, the file on your disk is byte-for-byte the file that was stamped. **That is the entire tamper check, and it does not require a Bitcoin node** — a node is only needed to confirm *which block* the stamp landed in.

**To confirm the block without a node**, read it off the proof and check it against explorers you pick yourself:

```
ots info timestamps/log.md.ots | grep -i bitcoin
#     verify BitcoinBlockHeaderAttestation(967920)
#     # Bitcoin block merkle root 8affbc39...
```

Look up that block height on **two** independent explorers and confirm the merkle root matches. Two, not one: a single explorer agreeing with `ots` proves nothing that a single bad source could not fake.

**A fresh stamp is pending for hours.** A proof created in the last few hours has no Bitcoin attestation yet — `ots info` shows only `PendingAttestation` lines, and that is not a failure. It is the anchor waiting to be mined. In that window the entry is defended by witnesses 1 and 3, not by Bitcoin.

---

## The entries

Every open and closed call lives in **[`log.md`](log.md)**, together with the price-field rule that
governs them. This file holds the rules, the verification instructions and the disclaimer; that file
holds the record. The two carry **separate proof chains** — see `timestamps/INDEX.md`.

---

## Scorecard

Published monthly. Every open and closed call, total return vs QQQ over the identical window, hit rate including losers.

No scorecard yet — the first one is published after the first full month of entries.

---

## What is not in this repository

Personal holdings and private research are **not** published here and never will be. Research on a position already owned is not a call, and mixing the two would corrupt this record before it starts. Only forward-looking, publicly-posted calls appear in this file.

---

## Disclaimer

*Standing disclaimer v1, in force from 2026-09-20. This text is versioned, not edited in place: changing a word publishes a v2 block below and leaves v1 standing, so any archived post can be matched to the disclaimer that was in force when it went out. The git history of this file is the proof of which was which.*

**Disclaimer.** This is not investment advice and nothing here is a recommendation to buy or sell any security. I am not a registered investment adviser, broker-dealer, or financial planner, and nothing published here is tailored to your situation, your holdings, or your risk tolerance. It is general commentary, published on a schedule, identical for every reader.

**Position disclosure.** I hold long equity positions, disclosed per-ticker whenever a specific name is discussed. I may add to, reduce, or close any position at any time, including immediately after publishing, and I am under no obligation to tell you when I do.

**The record.** Every call is timestamped before it's published and nothing is ever deleted — losers included, scored against QQQ from the same timestamp: github.com/shikabotai/pick-log

You are responsible for your own money. Do your own work.
