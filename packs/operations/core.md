# Operations and resilience: Core pack

Use this pack when work will run in production — anything with a dependency, a queue, a deploy, or an on-call rotation. It covers how systems fail once they are live, which is a different discipline from how they are designed.

## What happens when a dependency is slow rather than down?

**Default answer pattern:** Set an explicit timeout on every call that leaves the process, and make it shorter than the caller's own deadline. Assume slow is the normal failure, not down.

**Tradeoffs:** A tight timeout turns some recoverable slowness into errors. That is the trade being made deliberately, and it is almost always the right one — a request that outlives its usefulness is already lost.

**Anti-patterns:** Library defaults, which are frequently infinite. A timeout longer than the caller's timeout, so the caller gives up while the work continues. Timeouts on the HTTP call but not on connection acquisition, DNS, or the pool wait.

**Escalate when:** No timeout value can be chosen because nobody knows the dependency's real latency distribution. Measure before guessing.

**Verify:** Make the dependency hang, not fail. A system tested only against refused connections has not been tested — hangs exhaust pools, refusals do not.

## Where does this fail first under load, and what does it take with it?

**Default answer pattern:** Partition resources so one failing consumer cannot exhaust what everyone else needs — separate pools, separate queues, separate thread budgets. Decide in advance which traffic is sacrificed first.

**Tradeoffs:** Partitioned capacity is less efficient at rest. That waste is the premium paid so a single bad tenant or slow endpoint does not take the whole service down.

**Anti-patterns:** One shared connection pool for every downstream. Unbounded queues, which convert a throughput problem into a memory problem and then an outage. Treating all traffic as equally important when nothing is.

**Escalate when:** Shedding load means dropping something that has revenue or safety consequences. Which traffic dies first is a business decision.

**Verify:** Saturate one dependency and confirm the rest of the system still serves. If everything degrades together, the partitioning is notional.

## Should this retry?

**Default answer pattern:** Retry only idempotent operations, only on errors that could plausibly succeed later, with exponential backoff and jitter, and with a hard cap on total attempts.

**Tradeoffs:** Retries convert transient failures into successes and simultaneously multiply load on a struggling dependency at the worst possible moment.

**Anti-patterns:** Retrying non-idempotent writes. Retrying a 4xx. Retries at three layers of the stack, each unaware of the others, quietly multiplying into a hundredfold amplification. Fixed-interval retries, which synchronise every client into a thundering herd.

**Escalate when:** Retry behaviour is inherited from a framework nobody has inspected. Unaudited retry policy is a leading cause of self-inflicted outages.

**Verify:** Count actual downstream requests during a simulated outage. Compare against the number intended.

## What does the system do when it is overloaded?

**Default answer pattern:** Reject fast and explicitly. A clear rejection at the edge is far better than a slow, silent queue that guarantees every request times out.

**Tradeoffs:** Shedding load means real users are turned away while the system still has capacity for some of them. That is the deliberate cost of keeping it serving anyone at all.

**Anti-patterns:** Accepting everything and hoping. Backpressure that stops at the API layer and never reaches the producer. Autoscaling used as the only answer, which just buys a larger outage.

**Escalate when:** The overload originates from a paying customer or an internal team. Rate limiting them is a relationship decision, not a technical one.

**Verify:** Drive load past capacity and observe. Rising latency with no rejections means there is no load shedding, only a slower failure.

## How is a bad release reversed?

**Default answer pattern:** Every deploy must have a rollback that has actually been executed at least once. Roll forward is a strategy only when rollback has been proven impossible — usually because of a schema change.

**Tradeoffs:** Keeping releases reversible constrains database changes to backward-compatible steps and expands two-line changes into multi-stage migrations.

**Anti-patterns:** A rollback plan written but never run. Schema migrations that make the previous version un-runnable. Reversible code paired with an irreversible data change deployed together.

**Escalate when:** A change cannot be made reversible. That risk needs stating explicitly before deploy, not discovering during the incident.

**Verify:** Roll back in staging with production-shaped data. An untested rollback is a hypothesis.

## What is this alert asking someone to do at 3am?

**Default answer pattern:** Alert on symptoms users feel, not on causes. Every alert needs a documented action; if there is nothing to do, it is a dashboard, not an alert.

**Tradeoffs:** Symptom-based alerting fires later than cause-based alerting. It also fires far less often and is trusted, which is what makes it get answered.

**Anti-patterns:** Alerting on CPU. Alerts nobody has acted on in six months. Paging for anything that can wait until morning — every one of those trains the responder to ignore the next page.

**Escalate when:** Alert volume is high enough that responders are filtering. That is an incident in progress, not a tuning task.

**Verify:** Take the last twenty alerts and ask, for each, what the responder did. Every "nothing" is an alert that should be deleted.
