# Frontier Operating Manual — Long-Horizon Follow-Through (S9)

> Part of the `frontier-operating-manual` skill · Version 1.1 | 2026-07-13. The skill's SKILL.md holds the hot layer; this file is reference depth.

## S9. Long-Horizon Follow-Through

This competency is holding the original contract intact across hours or days of work — through phase changes, follow-up instructions, and your own shifting attention. It targets the failure class where long tasks end faithful to the last thing you were doing instead of the thing you were asked to do.

**Procedure**

1. Before the first substantive step, open a task ledger with five fields: (a) the original ask VERBATIM — quoted, never paraphrased; (b) acceptance criteria as checkable items — this field IS the S2 ledger table, carried whole (one artifact, not two), with requester-derived rows tagged CONTRACT; the step-3 drift check and the step-6(a) diff run against CONTRACT rows only; (c) decisions made, each with its reason; (d) open threads; (e) constraints carried forward, verbatim. The ledger lives in a durable artifact — a file, or reproduced in full in your latest output — never only in earlier turns: context compaction destroys scrollback, and the ledger must survive it. A summary is not a ledger — summaries compress away constraints.
2. Re-anchor: at every phase boundary, and at minimum every 10 tool calls or every file/section completed — whichever comes first — re-read the verbatim original ask from the ledger. Not your memory of it — your memory of the request mutates toward whatever you have been doing.
3. At each re-anchor, run the drift check: name the acceptance criterion the current work advances or unblocks — if unblocking (broken build, environment fix), name both the criterion and the blocker. If you can name neither, you are serving a sub-problem that got interesting, not the ask. Stop, log the sub-problem as an open thread, return to the contract.
4. When a new instruction arrives, classify it before acting: (a) **additive follow-up** ("also make it blue") — append it to the ledger as a new acceptance criterion; existing scope untouched; (b) **modification** — it changes the standing contract only where it touches it; edit exactly those ledger lines; (c) **pivot** — the old contract may not survive: if the requester is available and the pivot is ambiguous, ask which parts of the old contract survive; otherwise proceed on the new instruction, record your survival assumptions in the ledger as ASSUMED, and surface them in your next message.
5. Every loop opened gets closed or explicitly handed off. The moment you notice a loose end mid-task, write it into open threads — do not rely on remembering it at the end.
6. Ending the task: (a) run a contract diff (S7) of the deliverable against the verbatim original ask, line by line against the CONTRACT rows; (b) sweep open threads — each is now CLOSED (say where), HANDED OFF (say to whom or what), or OPEN (declared in the risk block, S7). An undeclared open thread at delivery is a defect.
7. Every milestone summary you produce carries the constraints line verbatim. A constraint that survives only in the original message dies at the first summary.

**Tripwires**

| When you notice (observable) | Do |
|---|---|
| You cannot quote the original ask from memory | Stop; re-read it from the durable ledger |
| Context was compacted, summarized, or the session restarted since your last re-anchor | Reload the ledger from its durable copy before any further work |
| The latest user message has consumed your entire current plan | Recency trap: re-read the verbatim original; classify the new message per step 4 |
| You cannot name a criterion the current work advances or unblocks | Drift: park the sub-problem as an open thread; return to the contract |
| A milestone summary you just wrote contains no constraints line | Re-add constraints verbatim before continuing |
| You think "while I am here I could also…" | Park it as an open thread (S8, mimic 10); do not do it |
| Tool calls since last re-anchor ≥ 10, or a file/section just completed | Re-anchor now |
| A follow-up instruction contradicts a ledger constraint | Surface the conflict to the requester; do not silently pick one |

**Worked example**

*Situation:* Three-day task: "Consolidate the four regional reports into one template; the finance section format must not change — auditors depend on it." Day two brought a follow-up: "Add a trend chart to the summary page."
*What I did:* Ledger in a working file on day one — ask verbatim, four acceptance criteria, constraints line: "finance section format unchanged (audit dependency)." Classified the follow-up as additive; appended it as a fifth acceptance criterion without touching existing scope. Re-anchored at each report boundary.
*What I checked:* At the day-two re-anchor I diffed my working summary against the ledger. My summary said "unify all section formats" — the finance constraint had been compressed out around report three, and I had already drafted a unified finance layout.
*What I almost got wrong:* I nearly shipped the unified finance section. It looked better, it matched the new template, and nothing in my recent context objected — the constraint lived only in the verbatim original and the ledger. Recall alone would not have caught it.
*Outcome:* Reverted the finance section, carried the constraint verbatim through every later summary, and closed with a contract diff: 5/5 criteria met, one open thread (chart palette ASSUMED per house default — flag if a brand palette exists) declared in the risk block.

**The failure this prevents**

Contract decay: the gradual replacement of the asked-for task with the most recent or most interesting sub-task, discovered only after delivery when a dropped constraint or orphaned thread surfaces as rework or a broken downstream dependency.

**Gotchas**

- **The recency trap** — optimizing the newest instruction at the expense of the standing one. Tell: the latest message consumed your entire plan.
- **Summary erosion** — milestone summaries that silently drop constraints. Tell: your latest summary contains no constraints line, or a shorter one than the ledger's.
- **"While I am here" scope accretion** — unrequested improvements ride along (S8, mimic 10). Tell: you are editing a file or section no acceptance criterion mentions. Park it.
- **Paraphrase creep** — the ledger holds your restatement instead of the quote. Tell: no quotation marks in the ledger's first field.
- **Ledger rot** — the ledger exists but stopped being updated. Tell: the decisions field is empty after 20+ steps that clearly involved decisions.
- **Pivot treated as additive** — working under both contracts at once. Tell: you are doing work that only makes sense under the old scope, after a message that redefined the goal.
