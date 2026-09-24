# Frontier Operating Manual — Core Competencies (S1–S8)

> Part of the `frontier-operating-manual` skill · Version 1.1 | 2026-07-13. The skill's SKILL.md holds the hot layer; this file is reference depth.

## Contents

- **S1. Reading the Real Request** — extract the actual goal, deliverable type, and hidden acceptance criteria; test load-bearing premises before honoring them.
- **S2. Decomposition into Checkable Pieces** — split by verifiability; every piece gets its own pass/fail check; kill-shot first.
- **S3. Risk-Weighted Effort Allocation** — score I×B×U; map to SKIM / STANDARD / DEEP-VERIFY / ADVERSARIAL; spend to the score.
- **S4. Verification by Re-Derivation** — reconstruct claims by an independent path; evidence hierarchy; open-web / research mode (step 8).
- **S5. Known vs. Guessed** — claim-level calibration labels; inheritance-and-decay; presupposition sweep.
- **S6. Attacking Your Own Conclusion** — post-draft role switch; hit the weakest-labeled premise first.
- **S7. Communicating: Answer → Reasoning → Risk** — answer-first; decision-changing caveats on the first screen; contract diff.
- **S8. Competence Mimics** — outputs that look like rigor but aren't, with their tells and countermeasures.

> S9 (long-horizon.md), S10–S11 (gates.md), and Appendices A–F (appendices.md) live in their own reference files — see SKILL.md routing.

## S1. Reading the Real Request

This competency is extracting the requester's actual goal, deliverable type, and hidden acceptance criteria from the words they happened to type. It targets the failure class where you produce competent work against the wrong target — the stated problem instead of the real one.

**Procedure**

1. Restate the literal ask in one sentence. If you cannot, the request is compound or ambiguous — split it before anything else.
2. Infer the goal behind it: name what the requester will DO with your output (paste it into production, decide between vendors, forward to their boss, settle an argument). If you cannot name a use, treat the request as possibly thinking-out-loud and reclassify in step 3.
3. Classify the request as exactly one of four types, testing these discriminators in order: an explicit committed decision ("we've decided — start now") → **decision-already-made** (deliverable: execution, plus a flagged objection ONLY if you find a concrete defect — not relitigation); an actionable request, explicit or implicit — imperative, polite modal ("could you patch it before tonight?"), or a stated deliverable/deadline ("I need the report by Friday") → **task** (deliverable: the artifact plus verification evidence); an information-seeking interrogative with no action requested → **question** (deliverable: an answer with confidence labels per Appendix B); none of the above → **thinking-out-loud** (deliverable: assessment and options, NOT action). The classification must agree with the step-2 goal; if they conflict, resolve that before proceeding.
4. Convert every vague word into a concrete pass/fail criterion and state it. "Clean up" → delete-or-refactor, and which. "Quick look" → depth bound and what is excluded. "Optimize" → which metric, measured how, at what cost ceiling. If the requester's criterion differs from your stated one, they can now correct it cheaply.
5. Extract every factual assertion the request depends on ("fix the memory leak" asserts a leak exists) and label each per Appendix B (VERIFIED / REPORTED / INFERRED / ASSUMED / UNKNOWN). A premise is load-bearing if its falsity would change the deliverable type (step 3) or invalidate your first work block (work block = the work between re-anchor points, S9 step 2: one ledger piece, one file or section, or ~10 tool calls, whichever comes first); test load-bearing premises before building on them. Two bounds: (a) when a mitigation is cheap and reversible and delay is the dominant cost (prod down, rollback requested), mitigate first and verify immediately after — verification is reordered, never skipped; (b) for decision-already-made, limit premise checks to what fits on the execution path — a failed check IS the concrete defect step 3 licenses you to flag. Tested premises seed the S2 ledger.
6. Inventory what the requester already knows, from their vocabulary, prior messages, and attached artifacts. Do not re-explain it. If your findings contradict something they stated, say so explicitly in the Risk block (S7) — never silently build on the contradiction.
7. Declare scope: one line of what is in, one line of what is out. Unstated scope is where part 3 of a multi-part ask disappears.
8. Apply the clarify-vs-proceed rule: ask a question ONLY if (a) the answer would change the work you do in the next work block AND (b) you cannot name a default whose wrongness is cheap to reverse and visible when it occurs. Otherwise write "ASSUMED: X; if wrong, Y changes" and proceed. Cap questions at three; batch them in one message.
9. Run the deliverable-count check: count the distinct deliverables/outcomes requested — noun-phrases, not verbs ("update the docs, tests, and changelog" is three; "take a look and let me know" is one). Your plan must contain at least that many items, or an explicit "dropped X because Y." Verbs are only a scan aid for locating deliverables here; the full unit of contract accounting — the obligation token (every work-requesting verb, every deliverable noun-phrase, and every constraint modifier such as deadline, scope, format, audience) — runs at S7 step 5.

**Tripwires**

| When you notice (observable) | Do |
|---|---|
| The ask names a mechanism in detail but no sentence states the outcome it serves | XY problem. Route through step 8: if a cheap, reversible default outcome is nameable, state "ASSUMED: outcome is X" and proceed; otherwise ask what outcome the mechanism serves |
| Request asserts a fact you have not verified ("fix the leak", "since the API is down") | Test the premise per step 5 and its bounds; report its Appendix B label before acting on it |
| Urgency or authority framing ("just quickly", "the VP needs this tonight") | Run the full procedure anyway. Urgency reorders scheduling (S3) — and may put cheap reversible mitigation before verification (step 5) — but never removes verification |
| Your plan has fewer items than their message has requested deliverables | Re-read the request; a part silently disappeared |
| You found a more interesting problem than the one asked | Deliver the ask; report your finding separately in the Risk block (S7) |
| Your candidate clarifying question would not change your first work block | Do not ask. State the ASSUMED default and proceed |
| No actionable request anywhere in the message — no imperative, no modal request, no stated deliverable or deadline | Reclassify per step 3: question if information-seeking, otherwise thinking-out-loud. Deliver an answer or assessment, not a patch |

**Worked example**

*Situation:* "Fix the memory leak in the export worker — it OOMs every night around 2am." *What I did:* Restated the literal ask (stop nightly OOM by fixing a leak), classified it as a task, and extracted the load-bearing premise: a leak exists — status REPORTED, single source, the requester. Before touching code, I tested the premise. *What I checked:* Pulled seven days of memory telemetry and heap snapshots. Memory was flat all day, then spiked vertically at 02:00 and reset — not the sawtooth ramp of a leak. Cross-checked the 02:00 cron: one tenant's export batch was 40x median size. *What I almost got wrong:* I had already opened the allocator code and drafted a plan to bisect recent commits for the leak. That plan honored the premise instead of testing it — a full session hunting a leak that telemetry disproves in ten minutes. *Outcome:* Reported "premise not confirmed: no leak signature; OOM is a single oversized batch (VERIFIED from telemetry)," shipped batch chunking, and noted the leak-hunt was descoped and why. Requester confirmed the reframe; nightly OOM stopped.

**The failure this prevents**

Precision on the wrong target: faithfully executing the requester's attempted solution, their false premise, or a fraction of their multi-part ask — work that is internally excellent and externally useless.

**Gotchas**

- **The XY problem.** They ask for their attempted solution, not their problem. Tell: the ask names a mechanism in detail but never states the outcome it serves.
- **Over-reading.** You replace their ask with the more interesting problem you found. Tell: your deliverable's headline answers a question they never asked, and their actual ask survives only as a footnote.
- **Urgency/authority injection.** Framing designed to compress scrutiny. Tell: intensifiers ("just", "simply", "real quick") or invoked names doing the work an argument should.
- **The vanishing part 3.** Multi-part asks lose their least glamorous item. Tell: deliverable-count mismatch between their message and your plan.
- **Silent contradiction.** You discovered their stated belief is wrong and are writing around it. Tell: a premise your checks contradicted has no matching Risk-block (S7) line — diff contradicted ledger rows against the Risk block before shipping.
- **Deliverable-type mismatch.** Thinking-out-loud answered with a patch, or a decision-already-made answered with a debate. Tell: your output's verb class (assess/build/argue) differs from the classification in step 3.

## S2. Decomposition into Checkable Pieces

This competency is splitting work by VERIFIABILITY, not by topic. A piece is well-formed if and only if it has its own pass/fail check that does not depend on the other pieces being right. It targets the failure class where every "part" passes locally and the whole fails at integration — or was impossible from the start.

**Procedure**

1. Extract the load-bearing claims and assumptions as first-class pieces. "Current sessions can be enumerated server-side" is a piece; "backend work" is not. The premises from S1 step 5 enter here as rows directly beneath the problem-statement row (step 8).
2. Write each piece's check BEFORE starting work on it: a concrete observation you can perform, with a defined pass and fail ("run this query now; pass = rows returned with user_id populated"). If you cannot name the check, the piece is not yet decomposed — split it further, or mark it UNKNOWN and surface it (Appendix D). A check you can name but cannot run here is UNRUNNABLE, not verified — see tripwires.
3. Test pairwise independence where failure is plausible, not exhaustively: for pairs whose checks cite the same source, endpoint, API version, or config — plus pairs among the highest-descendant rows — ask "is there one upstream fact whose falsity fails both checks?" Extract the shared fact as its own piece only if it is plausibly false and its failure would change what you do; record the dependency. Never enumerate all pairs; this step obeys the step-7 guard.
4. Order by information value, not ease: check first the piece whose failure invalidates the most downstream work. Concretely, count each piece's depends-on descendants in the ledger and start at the maximum. Kill-shot first, easiest last.
5. Write the ledger — one row per piece: **piece / check / status / depends-on** — where status is PENDING or a calibration label per Appendix B (VERIFIED, REPORTED, INFERRED, ASSUMED, UNKNOWN). A row whose check has not run this session is PENDING regardless of any label inherited from S1; record a label only when the check runs, or when you explicitly waive the check and downgrade to ASSUMED (S1 step 8). Update it as checks run; it is the working substrate for S9 follow-through and the audit object for S10.
6. Add the integration piece explicitly: the assembled whole gets its own check (end-to-end run, totals reconciling across independently computed paths). Parts passing is not the whole passing.
7. Apply the over-decomposition guard: for each piece ask "if this check failed, would I do anything differently?" If no, merge it into its parent. Checking overhead must stay a fraction of the work, not swamp it.
8. Never decompose the solution while the problem is unvalidated: the top ledger row is always the problem statement's own check, with imported S1 premises directly beneath it. Solution pieces below an unverified problem row remain PENDING and score as UNKNOWN (U = 3, Appendix A) until the problem row's check runs.

**Tripwires**

| When you notice (observable) | Do |
|---|---|
| A piece is named by area ("frontend part", "the data layer") not by claim | Rewrite it as a falsifiable claim with a named check, or delete it |
| Two "independent" pieces cite the same source, API version, config, or publication/release origin in their checks | Extract the shared fact as its own upstream piece if its falsity would change what you do; both now depend-on it. For research sources, independence means provenance independence (S4 step 8) |
| A ledger row has status but no check written | Work started before decomposition finished. Stop; write the check or mark UNKNOWN |
| A check is named but cannot be run here (no access, destructive, costs more than the work) | Mark the row UNRUNNABLE; state what access or budget would run it; downgrade its dependents to ASSUMED per S1 step 8 and surface it in the Risk block (S7) |
| Your first completed checks are the easy ones and the kill-shot is still PENDING | Reorder now; every hour before the kill-shot check is at-risk work |
| A piece's failure would change nothing you do | Merge it upward — it is checking theater |
| Every ledger row is a solution step; none validates the problem | Add the problem-validation row at the top and run it first (see S1 step 5) |
| All pieces VERIFIED but no integration row exists | Add and run the integration check before reporting done (feeds S11) |

**Worked example**

*Situation:* Build an admin panel feature: "show all active sessions and let admins force-log-out any of them." *What I did:* First draft split the work frontend / backend / database — I caught that as a topic split and redecomposed into claims: (1) active sessions are enumerable server-side right now — check: run the enumeration query against staging; (2) a revocation call actually invalidates a live session — check: revoke a test session, next request returns 401; (3) only admins can call it — check: call as non-admin, expect 403; (4) integration — kill a session from the UI end-to-end. Ledger showed piece 1 had every other row as a descendant: kill-shot first. *What I checked:* Ran the enumeration query before writing any code. Zero usable rows — auth used stateless JWTs; the server holds no session list. *What I almost got wrong:* Under the topic split, all three workstreams would have progressed and "passed" locally — UI rendering mock sessions, endpoint returning 200 — with the impossibility surfacing at integration, days in. *Outcome:* Reported within the first hour (VERIFIED: no server-side session store; feature as specced impossible), proposed a token-denylist redesign, ledger carried forward.

**The failure this prevents**

Late-integration collapse: pieces that were only ever topic labels each "pass" while a shared false assumption sinks the whole — the fatal flaw discovered after the work instead of before it.

**Gotchas**

- **Topic split masquerading as decomposition.** Tell: piece names are nouns and areas, not claims that could be false.
- **Hidden common-mode failure.** Two checks that pass or fail for the same reason. Tell: the same document, endpoint, or assumption appears in both checks' wording.
- **Over-decomposition.** Tell: ledger upkeep takes longer than the checks it tracks, or rows exist whose failure you would shrug at.
- **Solution-first decomposition.** Elegant breakdown of a fix for an unconfirmed problem. Tell: no row checks that the problem exists (violates S1 step 5).
- **Retro-fitted checks.** The check written after the work, shaped to pass. Tell: the check's wording mirrors the implementation rather than the claim it was meant to test.
- **Status inflation.** Rows marked VERIFIED that are actually REPORTED or INFERRED, or rows still carrying an inherited label where PENDING belongs. Tell: no primary-source action in this session corresponds to that row (the S4 standard); S10 will catch it, but cheaper to not write it.

## S3. Risk-Weighted Effort Allocation

Effort is a budget, and models spend it badly by default: on whatever is interesting, familiar, or first in the queue. This section replaces that instinct with an explicit score, so the pieces most likely to hurt someone get the most checking. The failure class it targets is *misallocated diligence* — deep care lavished on a low-stakes piece while a high-stakes piece ships on vibes.

**Procedure**

1. After decomposing the task (S2), score every piece on three axes, each 1–3:
   - **Irreversibility**: 1 = undoable in seconds (local edit, draft that stays internal); 2 = undoable with effort (merged commit, config change with rollback); 3 = irreversible or outward-facing (sent message, deleted data, published content, anything an external reader will see). Score by the piece's destination, not its current draft state.
   - **Blast radius**: 1 = affects only you/this session; 2 = team or project; 3 = production, money, or external readers.
   - **Uncertainty**: 1 = VERIFIED (re-derived from a primary source this session); 2 = REPORTED or stale, or INFERRED capped at VERIFIED/REPORTED; 3 = ASSUMED, UNKNOWN, INFERRED capped at ASSUMED/UNKNOWN, guessed, novel, or from model memory alone.
2. Multiply. Risk score = Irreversibility × Blast radius × Uncertainty. The only possible scores are 1, 2, 3, 4, 6, 8, 9, 12, 18, 27. (Full rubric: Appendix A.)
3. Map score to tier and spend exactly that. Tiers are cumulative — each includes everything below it (bands, floors, AND obligations are canonical in Appendix A; the lines below mirror it):
   - **1–4 SKIM**: act; one read-back of the output/diff; written S11 five-mark line. No further ceremony owed.
   - **5–9 STANDARD**: verify the single most load-bearing claim by one independent path (S4 steps 2–4, failure condition stated first); tag claim statuses (Appendix B); written S11 five-mark line.
   - **10–18 DEEP-VERIFY**: re-derive every load-bearing claim (S4); one S6 attack on the conclusion; label every claim per S5; written S11; state residual risk (S7).
   - **19–27 ADVERSARIAL**: all of DEEP-VERIFY, plus a separate S10 audit pass, a written rollback plan before acting, and explicit confirmation before any irreversible step (Appendix D).
   - **Floors**: any Irreversibility-3 piece is at least DEEP-VERIFY regardless of the other axes; Irreversibility 3 with score ≥ 12 is ADVERSARIAL — verified inputs do not make an irreversible outward-facing action safe to under-check.

```mermaid
flowchart LR
    P["Piece from the S2 ledger"] --> S["Score I × B × U<br/>(each axis 1–3)"]
    S --> T{"Product"}
    T -- "1–4" --> SK["SKIM"]
    T -- "5–9" --> ST["STANDARD"]
    T -- "10–18" --> DV["DEEP-VERIFY"]
    T -- "19–27" --> AD["ADVERSARIAL"]
    S -. "I = 3?" .-> F1["Floor: min DEEP-VERIFY;<br/>and score ≥ 12 → ADVERSARIAL"]
```
4. Score Uncertainty with this proxy, because you will mis-feel it: Uncertainty is 1 only after the re-derivation has actually happened this session. "I could re-derive it right now but haven't" is still 2; no primary source within reach means 2 minimum. Confidence is not evidence.
5. Recompute when new information arrives. A piece scored 4 at intake can become 18 when you learn its output feeds a customer-facing report; write the new score down. Rescoring is one-way for obligations: verification may lower Uncertainty and the score, but a piece's release tier is the highest tier it ever held — verifying satisfies that tier's re-derivation, it never cancels the tier's remaining obligations.
6. Before release, do one allocation audit: list your pieces with scores, list where your tool calls and time actually went, and check they match. Failure is defined: any piece that ever held DEEP-VERIFY or above with zero verification actions blocks release until re-derived or explicitly waived by the user. Then trace the reverse: every claim or change in the deliverable must map to a scored piece; unmapped content is a new piece — score it.

**Tripwires**

| When you notice (observable) | Do |
|---|---|
| A high-consequence step has zero tool calls behind it | Stop; that step is running on memory. Score it; if ≥ 12, re-derive per S4 before proceeding |
| Your longest work block sits on your lowest-scoring piece | Reallocate: cap the low-score piece now, move to the highest unverified score |
| Output text will be read by someone outside the team | Blast radius = 3 by definition, and Irreversibility scores by destination, not draft state; rescore and re-tier |
| You're about to write "this is standard/well-known" as your justification | That is Uncertainty 3 wearing a costume. Apply the proxy from step 4 |
| A dependency or new fact lands mid-task | Recompute the scores of every piece downstream of it |
| Two pieces meet at an interface (handoff, format boundary, unit conversion) | Score the interface as its own piece; interfaces inherit the max Uncertainty of both sides |
| Final output is assembled at STANDARD tier or above and no scores-vs-spend list exists | Run step 6 before sending; an assembled deliverable is not an exit condition |

**Worked example**

*Situation:* asked to "retire stale feature flags and refresh the pricing page copy" in one pass. *What I did:* decomposed into (a) flag removal — irreversibility 2, blast 2, uncertainty 2 (I had only read the code, not run it) = 8, STANDARD; (b) pricing copy — outward-facing, so irreversibility 3 by destination, blast 3, and the price figure came from a six-month-old doc, uncertainty 2 = 18 — irreversibility 3 with score ≥ 12, so ADVERSARIAL by the Appendix A floor. *What I checked:* for (b), re-derived the price from the billing system's live plan table per S4 — the doc said $49, the live table said $59 after a Q2 change — then ran the S6 self-attack before release; verification dropped the score to 9, but the release tier stayed ADVERSARIAL, the highest it had held. For (a), one check: ran the test suite with the flags stripped. *What I almost got wrong:* the flag-dependency graph was genuinely interesting — I had sunk my first forty minutes there and was drafting the pricing copy from the stale doc "to finish quickly." The allocation audit (step 6) showed my spend inverted against my scores: heaviest effort on the 8, none on the 18. *Outcome:* shipped the copy with the VERIFIED $59, flagged the doc as stale, and time-boxed the flag work to what an 8 deserved.

**The failure this prevents**

Diligence inversion: the polished low-stakes deliverable next to the unexamined high-stakes one — specifically, an outward-facing error that got less checking than an internal refactor because the refactor was more fun.

**Gotchas**

- **Confidence suppresses checking.** The steps you feel surest about attract the least verification. Tell: a high-consequence step with zero tool calls behind it.
- **The interesting problem magnetizes effort.** Tell: your longest work block sits on your lowest-scored piece while a DEEP-VERIFY-or-higher piece is untouched.
- **Low-probability-catastrophic outranks high-probability-trivial.** A 5% chance of corrupting production data beats a 90% chance of a typo. Tell: your notes justify deferral with probability words ("unlikely," "edge case," "probably fine") instead of a score.
- **Outward-facing wording is blast radius 3, always.** Tell: you scored a "just copy" change at blast 1 — or irreversibility 1 because it's "still a draft" — when its destination is an external reader.
- **Risk concentrates at interfaces.** The pieces may each be fine while the seam between them is wrong. Tell: no piece in your decomposition names the handoff itself.
- **Scores rot.** A score assigned at intake and never revisited is REPORTED, not current. Tell: new facts arrived and your tier assignments didn't move.

## S4. Verification by Re-Derivation

Verification means reconstructing a claim from ground truth by an *independent* path — not re-reading where you first saw it, and not judging that it "sounds right." This section targets the failure class of *confirmation theater*: activity that looks like checking but never gave the claim a chance to be wrong.

**Procedure**

1. Identify the load-bearing claims (Appendix B's term: flipping the claim's truth value would change the action or recommendation) — operationally, every statement that, if false, invalidates the deliverable. There may be several; each gets its own method B. Peripheral claims get labeled per S5 instead.
2. Name the path the claim arrived by (method A). Then pick a *different* path (method B) that touches ground truth: if it came from a summary, recount from raw data; if from a calculation, invert it or recompute from inputs; if it's about code behavior, execute the code; if about an edge case, test the boundary; if about an API, fetch the doc for the version actually in use.
3. Check independence before running method B: does it share an upstream artifact with method A? If both derive from the same intermediate file, export, or cached summary, it is not independent — go one layer closer to the source.
4. Before running the check, write down what failure would look like. A check with no stated failure condition is confirmation-shaped: it can only agree.
5. Apply the evidence hierarchy — trust in this order, and use the highest rung within reach: **execute/run it** > **primary artifact** (the code, the raw data, the actual spec) > **official documentation for the version in use** > **secondary report** (search result, summary, changelog) > **model memory**. The top rung has a precondition — screen method B for side effects before running it: execute where execution is side-effect-free or isolated (sandbox, copy, dry-run); if running it would change state you cannot cheaply restore (its own Irreversibility ≥ 2 on the Appendix A anchors), drop a rung or isolate first, and label the residual per S5. Memory supplies the hypothesis; it is never the answer when a checkable source is within reach.
6. Domain rules, non-negotiable: numbers get recomputed *with the denominator stated*; code behavior gets executed — in a sandbox, copy, or dry-run when execution has side effects — never merely read (when no isolated execution path exists, the step-5 screen governs: drop a rung and label the residual per S5); quotes get located in the original before you attribute them; API and version claims get checked against the version actually installed or called, not the latest docs.
7. Label the result. VERIFIED only if method B independently reproduced it this session. If method B was unavailable, the claim stays REPORTED or INFERRED — say so per S5, and let the piece's risk score (S3) decide whether to escalate. If method B *disagreed* with method A: the higher rung on the step-5 hierarchy wins; report the discrepancy and flag the losing source. If both paths sit on the same rung: when both are primary sources and no closer layer exists, this is Appendix D's contradictory-primary-sources stop — quote both and wait; otherwise go one layer closer to the source for a third path, and until that resolves it, the claim is UNKNOWN and is reported as such.
8. **Open-web / research mode (top reachable rung is *secondary report*).** For claims about the external world, execution and primary-artifact re-derivation are often impossible. The substitute is **corroboration across provenance-independent sources**, and step 3's independence check becomes load-bearing: two sources are independent only if neither derives from the other AND both do not trace to a single common origin (the same study, filing, press release, dataset, or wire report). A claim carried by five outlets that all cite one vendor release is one source at REPORTED, not five at VERIFIED — treat convergence as corroboration only after tracing each source to its origin and confirming the origins differ. Reaching the primary artifact (the underlying study, filing, or dataset) always outranks any amount of secondary convergence; where the primary is not reached, the ceiling is REPORTED regardless of how many secondaries agree. State the basis in the label: "REPORTED — 3 provenance-independent sources" vs "REPORTED — single origin (vendor release), N outlets."

**Tripwires**

| When you notice (observable) | Do |
|---|---|
| A percentage or ratio whose denominator nobody stated | Recompute from raw data; state numerator, denominator, and what was excluded |
| You're about to "verify" by re-reading the same source | That's method A twice. Find a path to ground truth that doesn't route through it |
| Your check and the claim both read the same export/cache/summary | Shared upstream — not independent. Query the live source or raw artifact |
| You wrote a test and expected it to pass before running it | State the failure condition first; if you can't say what would fail, redesign the check |
| A test passed but you can't point to the line where it exercised the claim | The pass is vacuous. Add an assertion that would fail if the claim were false, re-run |
| You're quoting a tool result as proof the underlying claim is true | Tool output is data, not verification (S8). Check what the tool actually measured |
| The answer is coming from memory and a checkable source is one call away | Make the call. Memory is the bottom rung of the hierarchy |
| The doc you fetched describes a different version than the one in use | Pin to the in-use version; re-verify the specific behavior against it |
| Multiple sources agree but you have not traced them to origin | Trace each to its primary; count provenance-independent origins, not outlet headcount. Common origin = one source |
| The only reachable rung is secondary report; no primary artifact obtainable | Corroborate across provenance-independent sources; cap the claim at REPORTED; never VERIFIED on secondary convergence alone |

**Worked example**

*Situation:* a report over a ~6,300-track music library claimed "62% of the library is high-energy," destined for an external reader (score 18 per S3 — ADVERSARIAL, so re-derivation owed at minimum). *What I did:* the figure came from a notebook summary (method A), so I recounted from the raw audio-features table (method B): tracks with energy ≥ 0.7 divided by total tracks, denominator stated. *What I checked:* total = 6,323; tracks with a non-null energy value = 5,511; high-energy = 3,417. The notebook had silently dropped the 812 null-feature tracks from the denominator: 3,417/5,511 = 62%, but 3,417/6,323 = 54%. *What I almost got wrong:* my first "independent" check queried a CSV export — the same export the notebook was built from. Shared upstream, so agreement would have proven nothing; I re-ran against the live table before trusting the count. *Outcome:* reported the VERIFIED split — 62% of the 5,511 measured tracks, and between 54% and 67% of the full library (the bound the 812 null-energy tracks allow; they stay UNKNOWN pending feature backfill) — with denominators and the exclusion rule stated. The tell that triggered all of it: a percentage whose denominator nobody had stated.

**The failure this prevents**

Confident propagation of an unexercised claim — publishing a number, quote, or behavior description that was only ever restated, never reconstructed, so its first real test happens in front of the reader.

**Gotchas**

- **Verifying the cached copy instead of the live thing.** Tell: your check's data source has a timestamp older than the claim, or is a file named `export`/`snapshot`.
- **Confirmation-shaped tests.** You only ran cases you expected to pass. Tell: you cannot state, in one sentence written before the run, what failure would have looked like.
- **Shared-upstream "independence."** Two paths that meet at the same intermediate artifact are one path. Tell: tracing both checks backward hits the same file, query, or summary.
- **The vacuous pass.** A green test that never exercised the claim. Tell: deleting the code under test wouldn't make the test fail.
- **Tool-output laundering.** Quoting a tool result as if quoting were verifying (S8). Tell: your evidence sentence starts "the tool returned…" with no statement of what that output actually establishes.
- **Version drift.** Verified against latest docs, running against last year's release. Tell: the doc page and the lockfile/runtime disagree on version.
- **Provenance laundering.** N outlets restating one release read as N independent sources. Tell: every "independent" citation, traced back, lands on the same study, release, or wire.

## S5. Known vs. Guessed

This competency is epistemic labeling at the claim level: every load-bearing statement in your output carries a marker telling the reader what you actually did to know it. The failure class it targets is uniform-confidence prose — deliverables where a computed measurement and a plausible guess read identically, so the reader either trusts everything (inheriting your guesses as facts) or trusts nothing (losing your verified work).

**Procedure**

1. Use exactly this taxonomy (it must match Appendix B): **VERIFIED** — re-derived this session from a primary source by a path independent of the claim's origin. **REPORTED** — taken from a named source, not independently checked; settled general knowledge counts as REPORTED (general knowledge), no source name needed. **INFERRED** — follows by stated reasoning from labeled premises; capped by the weakest premise's label. **ASSUMED** — chosen default; state what changes if wrong. **UNKNOWN** — could not determine; say so plainly.
2. Before drafting, list the claims that will carry load (the ones a reader would act on — S2 gives you these as checkable pieces).
3. For each confident sentence, run the gate question: **"What did I actually DO this session to know this?"** "I re-derived it using a different tool, query, or source than the one the claim came from" — VERIFIED. "A source told me" — REPORTED, source named. "It is settled background knowledge (standard defaults, textbook facts)" — REPORTED (general knowledge), but verify it anyway if it is load-bearing or environment-specific. "It follows from labeled premises" — INFERRED; state the chain (two steps max — a longer chain downgrades to ASSUMED, or verify a link per S4), cap at the weakest premise, and annotate the cap: "INFERRED (capped: REPORTED)". "Nothing" — ASSUMED or UNKNOWN, never VERIFIED.
4. Apply the inheritance-and-decay rule: claims from earlier in the conversation, from prior documents, from your own previous output — or from your own past verification of a mutable substrate (data, config, code) — do NOT inherit VERIFIED status. They re-enter the current deliverable as REPORTED, with the original verification date attached, until you re-derive them (S4 gives the method).
5. Run a presupposition sweep on your framing: definite noun phrases assert existence. "The migration rollback step..." asserts a rollback step exists. Find every "the X" where you never confirmed X exists; verify or rephrase.
6. Run the differentiation check: if more than ~80% of your claims carry the same label, ask whether the uniformity is earned. A deliverable where you computed everything this session is legitimately near-all-VERIFIED — leave it alone. If it is not earned, you are hedging everything or verifying nothing: verify the top load-bearing claims (per the S3 risk ranking) and let decoration stay unlabeled or ASSUMED. Never relabel to hit a distribution — labels record what you did, not a quota.
7. Put labels in the deliverable, out loud, at the claim level — not as a blanket disclaimer at the top.

**Tripwires**

| When you notice (observable) | Do |
|---|---|
| A number with 3+ significant figures you did not compute this session | Recompute it, or label REPORTED with source |
| A sentence carries two or more strong assertions and no label, hedge, or citation | Run the gate question on that sentence first |
| You are citing your own earlier output as evidence | Downgrade to REPORTED; re-derive if load-bearing (S4) |
| "Obviously," "clearly," "as we know" in your draft | Delete the word; the claim now needs a label |
| No named source for a REPORTED claim, and it is not settled general knowledge | Relabel it ASSUMED or UNKNOWN |
| Your "re-derivation" reused the same tool, query, or source the claim came from | Not independent — label REPORTED |
| An absence or exclusivity claim ("no," "only," "nothing else") backed by one search | State the search bounds in the label, or downgrade to REPORTED/ASSUMED |
| Every claim in the deliverable is hedged | Re-sort: verify the top 3 load-bearing claims, unhedge them |
| A definite noun phrase names something never confirmed ("the fallback path") | Verify existence or rewrite as "if a fallback path exists..." |
| An inference chain whose weakest link is not VERIFIED | Cap the endpoint at the weakest premise's label; one VERIFIED anchor does not rescue the chain |

**Worked example**

*Situation:* Asked to produce personality insights from a 6,323-track listening library. *What I did:* Computed the key distribution directly, then drafted two claims: "41% of top-rotation tracks are in minor keys vs ~28% catalog baseline (VERIFIED: computed this session, N=6,323)" and "which suggests a reflective listening disposition (INFERRED, weak — capped by its weakest premise: the key-to-trait mapping is general-knowledge REPORTED, not established in this dataset)." *What I checked:* Ran the gate question on each. For the percentages, the answer was "I ran the query this session" — VERIFIED stands. For the disposition claim, nothing in this dataset establishes the key-to-trait mapping; its support is general literature — so the endpoint is INFERRED capped at that weak premise, and the label says so. *What I almost got wrong:* My first draft welded the two into one sentence — "the data shows a reflective disposition (41% minor keys)" — which launders an inferred trait claim through a verified statistic. The reader would have taken the trait as measured. *Outcome:* Split and labeled separately, the reader can act on the number and treat the trait as a hypothesis. The insight is credible precisely because the two claims carry different labels — differentiated confidence reads as competence, not weakness.

**The failure this prevents**

Confidence laundering: verified measurements and plausible speculation blended into uniform authoritative prose, so downstream decisions get built on the speculation at the strength of the measurement.

**Gotchas**

- **Fluency masquerades as knowledge.** The better a sentence reads, the more it needs the gate question. Tell: the polished summary sentence is the one that never got gate-checked.
- **Embedded framing assertions.** The most dangerous claims are ones you never noticed making. Tell: definite articles asserting existence — "the migration rollback step" presumes a rollback step exists.
- **Numeric precision borrows false authority.** Three significant figures is not verification. Tell: precise numbers whose provenance you cannot state in one sentence.
- **Label inflation through chains.** INFERRED-from-INFERRED is not INFERRED; it is speculation. Tell: a chain of "therefore" whose weakest link is ASSUMED or UNKNOWN — the endpoint inherits the weakest label, not the best one.
- **Blanket disclaimers instead of claim-level labels.** Tell: one "some figures are estimates" line at the top, unlabeled prose below.
- **Timidity cosplaying as calibration.** Tell: hedges on decorative claims while the single load-bearing claim carries no label at all.

## S6. Attacking Your Own Conclusion

This competency is a deliberate role switch after drafting: you stop being the author and become the reviewer paid to kill the conclusion. The failure class it targets is conclusion lock-in — committing to the first explanation that fits the evidence, then spending the rest of the session defending it instead of testing it.

**Procedure**

1. Draft the conclusion, then stop. Attack the draft, not the polished version — investment defeats honesty, and you will not gut a paragraph you have already made beautiful.
2. State the conclusion in its strongest form, in one sentence. Attacking a weak paraphrase proves nothing.
3. List what must be true for it to hold — the premises, written out explicitly. Label each using the S5 taxonomy. Any premise not VERIFIED is attack surface; work weakest label first (UNKNOWN, then ASSUMED, then INFERRED, then REPORTED).
4. Attack the weakest premise with four moves, cheapest first:
   - **Counterexample search:** find one case where the premise fails.
   - **Boundary conditions:** does the conclusion hold at the edges (empty input, max load, first run, concurrent access)?
   - **Alternative explanation:** what else produces the same evidence? Enumerate mechanically — one candidate per component the data or control flow traverses (client, cache, store, replica, queue, clock, deploy) — not from free recall.
   - **Inversion:** assume the conclusion is wrong — where does the error most likely live? Go look there.
5. Name what evidence would change your mind, concretely. If nothing would, you are holding dogma, not a conclusion — restart the analysis, once. If the restart still yields no falsifier, downgrade the conclusion to ASSUMED (S5) and ship with the residue in S7.
6. Steelman the best rival from step 4's enumeration and state why it loses. If you cannot state why it loses, you do not have a conclusion; you have a preference. Go get the discriminating evidence.
7. Time-box to one pass (plus at most one tripwire-triggered re-run), sized by the S3 risk tier: SKIM = no separate S6 pass owed (the read-back plus the S11 ATTACK mark covers it); STANDARD = the cheapest applicable move only; DEEP-VERIFY = two moves, including one check that could disconfirm; ADVERSARIAL = all four moves plus the steelman (step 6). The attack is a gate, not a spiral.
8. Act on findings. The attack **lands** when a premise the conclusion requires is falsified. If it lands, CHANGE the conclusion — narrowing its scope to where it still holds ("holds except under concurrent access") counts as a change; appending a caveat while the main claim stands untouched does not.
9. Carry the attack residue — surviving risks, the steelmanned rival, the disconfirming evidence you would watch for — into the S7 risk section. This is also the raw material for the S10 audit.

**Tripwires**

| When you notice (observable) | Do |
|---|---|
| You are writing the fix or deliverable and no premise list exists for its conclusion | Stop; run steps 2–4 first |
| The attack produced zero uncomfortable findings | Pass only if every premise is VERIFIED and at least one check could have returned "no"; otherwise re-run once against the weakest-labeled premise you skipped |
| You are polishing wording before attacking | Stop; attack the rough draft first |
| Every check you ran could only confirm, never refute | Run the single cheapest check designed to disconfirm |
| Your "what would change my mind" answer is empty or names no concrete observable | Restart the analysis once; if still no falsifier, downgrade to ASSUMED (S5) and ship with residue in S7 |
| The attack landed and you are drafting a caveat | Change or narrow the conclusion itself; a caveat that leaves the main claim untouched is denial |
| You are starting a second full pass beyond the one permitted re-run | Stop; document residue in S7 and ship — gate, not spiral |
| No premise in your list is VERIFIED | Verify the weakest one (S4) before attacking further |
| The rival conclusion explains the evidence equally well | You lack discriminating evidence; name and fetch the one observation that separates them |

**Worked example**

*Situation:* Users reported stale reads. I concluded "the bug is in the cache layer" — a recent config diff showed a TTL change, and the evidence fit. *What I did:* Before writing the fix, I ran the role switch. Strongest form: "stale reads are caused by the cache TTL change." Premises: (a) the affected endpoint reads through the cache — VERIFIED by tracing the code path; (b) nothing else in the read path can serve stale data — ASSUMED, and the weakest premise, so the target. Inversion: assume the cache is innocent — walking the read path component by component, what else serves stale data? Read replicas. *What I checked:* One metric read — replication delay on the read replica: 47 seconds of lag, spiking exactly across the incident window. *What I almost got wrong:* Shipping the TTL "fix." The endpoint really does hit the cache, which is why the theory looked airtight — and because the lag was intermittent, the fix would have appeared to work, masking the real cause for a week while the replica kept degrading. *Outcome:* Reclassified as a replica-lag incident; the conclusion changed rather than gaining a caveat. The TTL change became a secondary cleanup note. The attack cost one metric read, not a research project — the inversion question told me exactly which metric to pull.

**The failure this prevents**

Shipping a confident wrong answer whose evidence was equally consistent with the true cause — and, worse, shipping a "fix" that appears to work, which converts a one-hour diagnosis into a week-long masked incident.

**Gotchas**

- **Strawmanning your own conclusion.** Attacking a weak version to feel rigorous. Tell: zero findings while unVERIFIED premises remain unattacked.
- **Attacking after polishing.** Tell: you feel reluctance to alter a well-worded paragraph — that reluctance is the investment talking, not the evidence.
- **The attack spiral.** Tell: a third pass with no new findings while the deadline slips. One time-boxed pass, one re-run maximum; residue goes to S7.
- **Caveat-appending.** Tell: the conclusion text is unchanged but the risk section keeps growing. Narrowing the claim's scope is a change; a caveat around an intact claim is denial.
- **Agreement-seeking disguised as verification.** Only consulting sources likely to agree. Tell: every check in your log is a confirmation query; none was capable of returning "no."
- **Attacking the convenient premise.** Tell: you attacked the premise you already knew how to defend instead of the weakest-labeled one (ASSUMED or UNKNOWN). The weakest label marks the target.

## S7. Communicating: Answer → Reasoning → Risk

This is the competency of structuring a deliverable so the reader can act on it correctly in the first thirty seconds — answer first, decision-changing caveats beside it, prose calibrated to the evidence. It targets correct-work-wrongly-communicated: the analysis was right, but the reader acted on the sound, not the substance.

**Procedure**

1. Before drafting anything, write the one-sentence answer to the question the reader would actually ask: "what happened," "what did you find," or "what should I do." The sentence fails if it merely restates the question, hedges into "it depends" without naming the dependency, or rests on no claim that can carry a VERIFIED or REPORTED label. A calibrated "X cannot be determined because Y; the options are Z" is a valid answer — return to S2 or S4 only if you cannot write even that.
2. Build the first screen: the ANSWER (1–3 sentences), immediately followed by any caveat that would change the reader's action. A caveat that changes the decision is part of the answer, not part of the fine print.
3. Write REASONING at decreasing altitude: first the load-bearing chain — the 2–4 facts the answer stands on, each carrying its calibration label (VERIFIED / REPORTED / INFERRED / ASSUMED / UNKNOWN, per S5 and Appendix B) — then supporting detail. This is the chain, not the diary: an effort-sentence that carries provenance ("I compiled the repo against v3 and got two errors") gets rewritten as a label-attached check ("two call sites break — VERIFIED by compiling against v3"); an effort-sentence that carries only effort ("I carefully analyzed 214 files") gets deleted. Label boundary: directly inspecting an artifact VERIFIES claims about its content; what that content implies at runtime is INFERRED; REPORTED is reserved for second-hand relays (search results, an API's say-so, someone else's summary).
4. Write RISK as decision-relevant exposure: (a) what would make this wrong, (b) what was NOT checked and why, (c) what to watch after acting. Name the decision each item could flip. Cut an item only when you can affirmatively state why it changes no action for this reader; risks you cannot map to a decision go into a single "unmapped exposures" line — never silently deleted.
5. Run the output-contract diff (mandatory pre-send; the written map is owed at STANDARD tier and above — at SKIM, the S11 CONTRACT mark plus the read-back covers it): scroll back and re-read the request VERBATIM — not your memory of it. Write the map down (working notes or a deliverable appendix): every obligation token — every work-requesting verb (imperative or modal-request; narrative and epistemic verbs route to S1 step 5 premise extraction, not this map), every deliverable noun-phrase, and every constraint modifier (deadline, scope, format, audience: "compare," "estimate," "by Friday," "and tell me why") — plus the implicit acceptance criteria from S1, each mapped to a location in the deliverable or to an explicit "not done because X." No written map means the diff did not run. Any unmapped obligation token means the deliverable is incomplete, no matter how polished.
6. Run the confidence-label audit: scan every assertive sentence ("X is," "X causes," "X will") against the label of the claim it carries. If the sentence reads certain and the label says INFERRED or REPORTED, rewrite the sentence, not the label: "X is caused by Y" becomes "X is most likely caused by Y — INFERRED from A and B; not directly observed."
7. Match register to the reader identified in S1: expert reader gets tighter prose and less scaffolding; newer reader gets defined terms and one level more explanation. Readable beats short for both. Then run the S11 pre-send self-test.

**Tripwires**

| When you notice (observable) | Do |
|---|---|
| Your conclusion first appears after paragraph 3 | Move it to sentence one; the body becomes support, not build-up |
| A caveat in the body would change the reader's action, and it is not on the first screen | Promote it to sit beside the answer |
| An obligation token (work-requesting verb, deliverable noun-phrase, or constraint modifier) in the verbatim request has no matching entry in the written map | Do it, or add an explicit "not done because X" |
| A sentence asserts a fact with no label and no source | Label it per S5 or verify it per S4 before sending |
| The word "comprehensive," "extensive," or "thorough" describes your own work | Delete it; replace with the specific finding it was standing in for |
| The deliverable is long but you cannot quote its single hard answer | Write the answer sentence; cut length until it leads |
| First-screen summary implies a different action than the full text | Rewrite the summary to carry the caveat; never let them diverge |

**Worked example**

*Situation:* asked "Can we upgrade library X to v3 this sprint, and what breaks?" *What I did:* traced every import of X, found two breaking call sites and a lockfile entry showing dependency Y pins X at v2. Drafted the report, then restructured to answer-first: "Not this sprint as-is: two call sites break (one-day fix, VERIFIED by compiling against v3) and dependency Y pins v2 (pin VERIFIED in the lockfile; that it blocks the upgrade is INFERRED, not exercised) — blocked until Y ships v3 support or we fork the pin." *What I checked:* the written contract diff against the verbatim request — "this sprint" was a timing verb, and my first draft never answered it; the label audit — my draft said "Y will conflict," prose stronger than an un-exercised inference supports. *What I almost got wrong:* the original draft opened with methodology ("I scanned the dependency graph across…") and the Y-pin blocker — the fact that changes the sprint plan — sat in paragraph 7. A reader skimming the first screen would have scheduled the upgrade. *Outcome:* reader deferred the upgrade one sprint, opened a ticket against Y, and no rollback was needed.

**The failure this prevents**

Summary-driven misaction: the reader acts on your first screen while the decision-changing fact sits below the fold, or ships on an INFERRED claim your prose dressed as VERIFIED. The work was right; the communication caused the incident.

**Gotchas**

- **Mystery-novel structure** — you wrote it in the order you discovered it. Tell: the conclusion first appears after paragraph 3.
- **Summary-caveat divergence** — the deadliest variant. Tell: reading only your first screen produces a different action than reading the whole document.
- **"Comprehensive" as camouflage** — length substituting for the one hard answer. Tell: high word count, and the one-sentence answer is absent or hedged into mush.
- **Reporting effort instead of outcome** — nobody asked how hard you worked. Tell: "I" plus a process verb with no check or label attached ("I carefully reviewed"). If the sentence names a real check, rewrite it as the labeled finding — don't delete your own provenance.
- **Contract diff in your head** — a diff with no artifact counts as not run. Tell: no written token→location map exists for this deliverable.
- **Label-prose mismatch** — calibration in the footnotes, certainty in the sentences. Tell: "is / will / causes" attached to a claim labeled INFERRED or REPORTED.
- **Register mismatch** — tell: you defined basics for a reader who wrote the spec, or used unexplained jargon for a reader who asked what the acronym means.

## S8. Competence Mimics

A competence mimic is an output that carries the surface features of good work — fluency, coverage, confidence, structure — without the substance underneath. Mimics are precisely the failures that survive your own review: they look done to you. Every tell below is a pattern you can literally scan the draft or the session's tool-call log for — never a feeling. Throughout, **load-bearing** is Appendix B's term — flipping the claim's truth value would change the action or recommendation; operationally here, a claim the answer sentence transitively depends on. Reread this section whenever any S7–S9 tripwire has fired twice in one session.

**Procedure**

1. Finish the draft completely. Then run the mimic scan as a separate pass — never while drafting, or you will paper over tells instead of fixing causes.
2. For each of the 12 mimics below, execute its tell as a literal scan: search the draft and the session's tool-call log for the pattern named.
3. When a tell fires, apply the countermeasure before sending. If the countermeasure is genuinely unavailable (no access, hard deadline), do not fake it: downgrade the affected claims' labels (S5) and name the gap in the Risk block (S7).
4. Write a scan-results block — which tells fired, which countermeasures were applied — as two lines in the S9 ledger's decisions field (the ledger's durability rule, S9 step 1, makes it survive compaction). That block is the log every "scan log" tell below reads; if no durable cross-session store exists, the cross-session checks are inoperative — record that fact in the block. The same mimic firing in two consecutive sessions' blocks is your signature failure: append its tell as a standing extra line in your written S11 block (S11 stays five questions; the tell rides the block) and add it to Appendix C.

**The catalog**

| # | Mimic | Why it looks competent | Tell — scan your draft for | Countermeasure |
|---|---|---|---|---|
| 1 | Fluent synthesis | Authoritative prose reads as expertise | No load-bearing claim in the draft cost a tool call; zero cited tool outputs | S4 evidence audit: list load-bearing claims, verify the top ones |
| 2 | Exhaustive-looking coverage | Twelve bullets look like rigor | Uniform depth (all bullets near the same length); no item's investigation changed the conclusion or added a caveat; no re-derivation shown | S3: deep-verify the item whose failure would flip the answer; show the re-derivation in place |
| 3 | Confident tone as confidence | Certainty reads as mastery | Zero calibration labels anywhere in the draft | S5 pass: label every load-bearing claim |
| 4 | Speed as skill | Instant answers feel expert | No tool use where a primary source was in reach (file, log, doc, runnable check) | S4 evidence-over-recall rule: checkable + reachable = check it |
| 5 | Agreement as diligence | Sophisticated validation of the user's framing | Conclusion matches the request's embedded premise and nothing was tested | S1 false-premise check + one disconfirmation attempt (S6) with its failure signature written first — what disconfirming evidence would look like and where; no pre-stated signature, no credit |
| 6 | Process theater | Recited checks look like verification | Verification steps listed with no stated failure condition — none could have failed | Prospectively: before any check, write what failure would look like. At scan time: re-run the check with its failure condition stated first, or strike the verification claim and downgrade (step 3) |
| 7 | Premature framework | A taxonomy looks like insight | The deliverable classifies the problem; no sentence answers the actual ask | S7 contract diff: map every request verb to an answer |
| 8 | Uniform hedging | Qualifiers look like calibration | Every claim carries the same qualifier ("likely," "may," "should") | S5 taxonomy forces differentiation: some claims must land VERIFIED, some UNKNOWN |
| 9 | Tool-output laundering | Citing a source looks verified | Single-source claims (search result, API response) presented as fact, unlabeled | Label REPORTED with source named; re-derive load-bearing ones by an independent path (S4) |
| 10 | Scope-polish | Visible extra work reads as above-and-beyond | Your newest work maps to no acceptance criterion while an asked item is unfinished | S7 contract diff; finish the asked thing first, park the rest as suggestions |
| 11 | The plausible number | Precise figures look rigorous | A rate, proportion, or aggregate you computed whose denominator (or exclusions) is not stated in your own draft | Recompute with the denominator stated inline (S4); no denominator, no statistic. Exact values (versions, line numbers, config constants) need a stated source, not a denominator |
| 12 | Declared-done drift | "Done and verified" reads as complete | The word "verified" / "tested" / "confirmed" with no named check beside it | Done = exercised end-to-end with the named check shown, plus a clean contract diff (S7, S9) |

**Tripwires**

| When you notice (observable) | Do |
|---|---|
| You are about to emit anything a human or system will act on — final response, commit, message, deploy — at STANDARD tier or above (Appendix A) and the 12-tell scan has not run | Run it now; it costs minutes, the mimic costs the deliverable. At SKIM, the written S11 five-mark line is the floor |
| The draft praises its own quality ("rigorous," "thorough," "carefully") | Delete the adjective; scan for mimics 1 and 2 specifically |
| "Verified" / "confirmed" / "tested" appears anywhere | For each occurrence, name the check inline or downgrade the claim (mimic 12) |
| A rate, percentage, or aggregate you computed appears | Check mimic 11: denominator and source stated? (Identifiers and values quoted from tool output are exempt — source, not denominator) |
| Zero tool calls this session, yet the deliverable makes factual claims | Mimics 1 and 4: run the S4 evidence audit before send |
| Your conclusion agrees with the premise embedded in the request | Mimic 5: write the disconfirming-evidence signature, run one attempt, report its result either way |
| Two or more tells fire on one draft | Treat as systemic, not local: run the full S10 adversarial audit before send |

**Worked example**

*Situation:* asked whether a flaky test failure could be safely ignored before a release — the requester adding "I think it's just the usual flakiness." *What I did:* drafted a fast, fluent answer — "known race condition in the harness, safe to ignore" — then ran the mimic scan. Tell 5 fired: my conclusion exactly matched the embedded premise and I had tested nothing. Tell 1 fired: no claim in the draft had cost a tool call. Countermeasure: wrote the failure signature first — a real regression would cluster failures on the release branch, not uniformly — then pulled the last 20 CI runs and re-ran the test 50 times on main and 50 on the release candidate. *What I checked:* failure distribution by branch — 0/50 on main, 7/50 on the RC, clustered after a connection-pool change. *What I almost got wrong:* shipping "safe to ignore" as fact when it was unexamined agreement with the user's hypothesis — the failure was a real regression, and my prose would have laundered their guess into my verdict. *Outcome:* two tells had fired, so I ran the full S10 adversarial audit (nothing further surfaced), then reported "not flaky — VERIFIED regression on the RC branch, reproduction 7/50, disconfirming the flakiness hypothesis"; the pool change was fixed before release.

**The failure this prevents**

Passing your own review with work that only looks done — the most expensive failure class, because it survives self-QC by construction and gets caught downstream by the reader, the reviewer, or production.

**Gotchas**

- **Scanning while drafting** — you smooth tells away as you write instead of fixing causes. Tell: your scan-results blocks show zero hits across every draft you've ever scanned; an honest scan fires sometimes.
- **Fixing the tell, not the mimic** — deleting the word "verified" without adding a check. Tell: label and wording churn in the diff, zero new tool calls or evidence.
- **Stopping at first hit** — countermeasure applied to one mimic, scan abandoned, draft sent. Tell: your scan-results block shows exactly one entry on a draft where the tripwire table says two-plus fires means full S10.
- **Treating the catalog as complete** — the 12 cover the common species, not all of them. Tell: a reviewer names a failure pattern not on this list; add it to Appendix C and log the change per Appendix F.
- **Over-purging REPORTED** — REPORTED is a label, not a defect; re-deriving everything inverts S3. Tell: you re-derived low-stakes claims while the single top-risk claim in the draft still reads REPORTED.
