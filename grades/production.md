# Production grade

Supersedes: `mvp` — resolve [`grades/mvp.md`](mvp.md)'s 6 items first. They are not repeated here; this gate assumes them passed.
Superseded by: `enterprise` — includes every item below by reference, never restated.

The bar: would an on-call engineer accept ownership of this at 3am. Not "does
it work" — that's MVP, resolved separately, not re-asked here — but "if this
pages someone, do they have what they need, and does the system fail in a way
that's survivable."

Mandatory domains: Requirements, Security, Testing, Observability, Cost/Performance, plus whichever domains the dynamic scan already selected. Mandatory packs: `operations`, `threat-modeling`, `testing-design`.

## Gate

Items that link to an existing pack card resolve **there** — read the card,
apply its default, verify with its check. Do not re-derive the reasoning in
this file; the card already has it. This is the same rule the gate itself
follows toward `mvp` above: an item is defined in exactly one place, and
every other file that needs it links rather than restates. That's what keeps
loading several files at once from producing two different answers to the
same question.

| # | Item | Resolves via |
|---|---|---|
| 1 | Timeouts on every call that leaves the process | `packs/operations/core.md` — "What happens when a dependency is slow rather than down?" |
| 2 | Retry policy is deliberate, capped, and audited | `packs/operations/core.md` — "Should this retry?" |
| 3 | The system rejects fast under overload instead of queuing silently | `packs/operations/core.md` — "What does the system do when it is overloaded?" |
| 4 | A rollback exists and has actually been run once, not just planned | `packs/operations/core.md` — "How is a bad release reversed?" |
| 5 | Every alert has a documented action; dashboards and alerts aren't the same thing | `packs/operations/core.md` — "What is this alert asking someone to do at 3am?" |
| 6 | Trust boundaries and attacker paths for this specific system have been named | `packs/threat-modeling/core.md` |
| 7 | The test suite would actually catch the regression that matters most, not just run green | `packs/testing-design/core.md` — "What makes this test worth its maintenance cost?" |

Items with no existing card — genuinely new territory, resolved here:

| # | Item | Resolved when | Escalate to user only if |
|---|---|---|---|
| 8 | A stated target for uptime, latency, or error rate exists, and what's built can plausibly hit it | one sentence, e.g. "99.5% uptime, p95 under 400ms" | the target itself, or whether it's realistic given the current design — that's an authority call, not an engineering one |
| 9 | Resource and cost at roughly 10x current expected load has been estimated, even roughly | a number exists, not a shrug | the acceptable cost ceiling |
| 10 | Backup exists *and restore has actually been executed once*, not just configured | a real restore ran and produced usable data | never — if it hasn't been tested, the honest state is "not verified," not an escalation |
| 11 | If this pages someone, a runbook exists — not the code, an actual "if X, do Y" document | confirmed present and named | never |
| 12 | The system can be turned off or degraded without a full redeploy — a kill switch, a feature flag, anything short of "revert and redeploy" | confirmed present | if none exists, whether that's acceptable for this system is the user's call |
| 13 | Anything stored has a stated retention and deletion policy, and deletion has been verified to actually delete | policy stated, deletion tested | the retention period itself, if it touches legal or compliance |
| 14 | Someone specific owns this when it breaks, and they know it | named, not implied | never — but if nobody does, say so explicitly rather than leaving it silent |

Fourteen items. Most of the linked ones (1–7) resolve from reading the repo
against the pack's default. Most of the new ones (8–14) are yes/no checks on
whether something exists and was actually exercised, not designed from
scratch — a runbook and a tested restore are artifacts to point at, not
essays to write.

The honest failure mode of this grade is marking something "resolved" because
it was *configured* rather than *verified*. Item 10 exists specifically
because "we have backups" and "we have restored from a backup successfully"
are different claims, and only the second one is the gate.
