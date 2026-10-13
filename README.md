# Frontier Operating Manual

An operating discipline for AI coding agents and the people who direct them: how to read the real request, decompose work into checkable pieces, spend verification effort in proportion to risk, label what is known versus guessed, attack a conclusion before shipping it, and stay anchored across long tasks.

It ships as a Claude Code skill. The skill file is the hot layer, about seventy lines. Four reference files hold the full procedures, worked examples and rubrics, about sixteen thousand words. A CLAUDE.md insert carries the standing rules into every session, whether or not the skill loads.

## Why it exists

Capable models fail on hard tasks in repeatable ways: answering a nearby easier question, treating a request's premise as fact, re-reading a source and calling it verification, sounding more certain than the evidence, drifting from the original ask over a long session. None of these is a knowledge gap. Each is a discipline gap under load. This manual encodes the discipline as standing rules with pass or fail checks, so a session can be held to them and audited against them.

## What is in it

| File | Holds | Load when |
|---|---|---|
| [`SKILL.md`](SKILL.md) | The operating loop, ten standing rules, risk tiers, calibration vocabulary, the five-question pre-send self-test, hard stops | Whenever the skill loads. This is the hot layer. |
| [`CLAUDE-md-insert.md`](CLAUDE-md-insert.md) | The always-on distillate: the ten standing rules with their anchors, the five-question self-test, the hard stops and the mimic self-scan | Every session, imported into `CLAUDE.md` (see Install) |
| [`references/core-competencies.md`](references/core-competencies.md) | S1 to S8: reading the request, decomposition, risk-weighted effort, verification by re-derivation, known versus guessed, self-attack, answer-first communication, competence mimics | Starting a hard task, or suspicious of a draft |
| [`references/long-horizon.md`](references/long-horizon.md) | S9: the ledger, re-anchoring, thread discipline | Tasks spanning hours or days, context compaction, drift |
| [`references/gates.md`](references/gates.md) | S10 the adversarial QC audit, S11 the self-test rules | Shipping outward-facing or irreversible work |
| [`references/appendices.md`](references/appendices.md) | A to F: the risk rubric, label rules, symptom library, escalation triggers, an evaluation harness, versioning | Exact anchors and thresholds |

### The ten standing rules, one line each

1. Restate the ask; count the deliverables.
2. Test load-bearing premises before honoring them.
3. Decompose by verifiability, not topic.
4. Score risk before spending effort: irreversibility times blast radius times uncertainty.
5. Never answer from memory when a checkable source is within reach.
6. Verification is re-derivation by an independent path.
7. Label every load-bearing claim: VERIFIED, REPORTED, INFERRED, ASSUMED, UNKNOWN.
8. Attack your own conclusion once, before polishing.
9. Answer, then reasoning, then risk.
10. Re-anchor on the long arc.

## Install

As a Claude Code personal skill:

```bash
git clone https://github.com/jtims/frontier-operating-manual.git ~/.claude/skills/frontier-operating-manual
```

The skill loads on its trigger vocabulary (hard problem, verify, audit, risk score, calibration, resume, handoff, session close) or by name with `/frontier-operating-manual`. It runs on any model. It was written so a capable model can inhabit it without fine-tuning.

To keep the standing rules in force in every session, not only when the skill triggers, import the insert from `~/.claude/CLAUDE.md` on a line of its own:

```text
@~/.claude/skills/frontier-operating-manual/CLAUDE-md-insert.md
```

To use the text outside Claude Code, read [`SKILL.md`](SKILL.md) first and pull the reference files as the routing table at its end directs.

## How it was built

The manual was produced under its own method: parallel drafts, adversarial review of each draft, revision, an editorial pass, a three-lens audit, fixes, and a re-audit, with every finding recorded. Version 1.2 extended the trigger vocabulary after observed driver-seat incidents; the rules did not change.

## What this demonstrates

- Governance as a runnable artifact rather than a policy document: rules with failure conditions, tiers with thresholds, a self-test that must be written down.
- Calibration discipline: a fixed vocabulary for how a claim is known, and a rule that prose can never outrun the label.
- Risk-proportional effort: a scoring rubric that decides how much verification a task earns before any work starts.
- Long-horizon operation: a ledger and re-anchoring protocol for sessions that outlive a context window.

## License

Creative Commons Attribution 4.0 International (CC BY 4.0). See [`LICENSE`](LICENSE). Attribution: jtims, github.com/jtims/frontier-operating-manual.
