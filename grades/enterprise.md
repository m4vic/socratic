# Enterprise grade

Supersedes: `production` — resolve [`grades/production.md`](production.md)'s 14 items first (which itself resolves [`mvp.md`](mvp.md)'s 6). None are repeated here; this gate assumes them passed.
Superseded by: — (top grade)

The bar: would this pass a large customer's procurement and security review,
survive an actual regional outage, and hold up under an audit — not "is it
reliable" (`production` already covers that) but "can the organization stand
behind the commitments it's making to people outside it."

Mandatory domains: everything `production` requires, plus Compliance. Mandatory packs: everything `production` requires; escalate `threat-modeling` and `data-systems` to their complete files if either loaded as Core.

## Gate

Every item below is new territory — nothing in `production` or the packs
covers it, because it's not about the system's engineering, it's about the
organization's commitments and exposure around the system.

| # | Item | Resolved when | Escalate to user only if |
|---|---|---|---|
| 1 | Disaster recovery has been drilled as a full failure, not just a restored backup — a stated RTO/RPO, tested against | a real drill happened and the actual recovery time was measured against the target | the RTO/RPO target itself — that's a business risk-tolerance call |
| 2 | If multi-tenant, one tenant's failure or load cannot degrade another's — isolation is stated and has been tested, not assumed | a concrete test exists: one tenant hammered, others measured | whether the isolation model is strict enough for the customers being sold to |
| 3 | An audit trail exists for who did what to production and customer data — append-only or tamper-evident, not just application logs that can be edited | confirmed present, confirmed someone other than the actor can read it | never |
| 4 | Changes to production go through a process with a record of who approved them, not just "someone with access pushed it" | the approval trail exists and was checked against a recent real change | never |
| 5 | A customer-facing incident communication path exists — status page, notification process, or equivalent — separate from the internal runbook `production` already requires | confirmed present and named | never |
| 6 | The list of third parties that touch customer data is documented, along with what each one can see | a real list exists, not "we'd have to check" | never |
| 7 | Data residency and regional storage constraints are stated, if any customer or regulation requires them | stated explicitly, even if the answer is "no constraint applies" | which constraints apply — that's a legal/compliance call, not engineering's to assume |
| 8 | The external SLA promised to customers has been checked against what `production`'s internal SLO (item 8 there) can actually deliver — they are not assumed to match | compared explicitly; a gap between promised and achievable is stated, not hidden | the promised number itself |
| 9 | A compliance sign-off path exists for whatever regime applies (SOC 2, GDPR, HIPAA, or none) | named, even if "none applies and here's why" | which regime applies, and whether the current state passes it — this is never an engineering default |

Nine items. Almost every one is a yes/no check on whether an artifact or
process exists and was exercised for real — a drill that happened, a trail
that was read, a list that was checked — not something built from scratch by
an engineering default. Several explicitly cannot be resolved by the agent
alone (3 of 9 have a real escalation path) because this grade is mostly about
commitments an organization makes, which is authority, not engineering.

Item 8 exists because "internal SLO" and "customer-facing SLA" are different
promises made to different audiences, and `production` only checks the
first — this grade is where the gap between them gets caught before a
customer does.
