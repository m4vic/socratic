# Designing Data-Intensive Applications: Core pack

Use this pack for stateful services, distributed data, queues, caches, migrations, retries, and recovery behaviour.

## What is the source of truth for this data?

**Default answer pattern:** Name one authoritative system of record and treat derived stores, indexes, caches, and analytics copies as rebuildable projections.

**Tradeoffs:** A single authority simplifies correctness; derived views improve read performance and availability but introduce lag and repair work.

**Anti-patterns:** Two writable databases without conflict rules, treating cache data as canonical, or allowing ownership to vary by code path.

**Escalate when:** Multiple writers are unavoidable, data crosses organisational boundaries, or conflict resolution changes product meaning.

**Verify:** Document each data entity's owner, write path, replication path, and rebuild procedure.

## What consistency does the user-visible operation actually require?

**Default answer pattern:** Start from the user invariant, then choose the weakest consistency guarantee that still protects it.

**Tradeoffs:** Stronger coordination reduces anomalies but adds latency, lower availability, and operational complexity. Eventual consistency improves resilience but needs clear stale-read behaviour.

**Anti-patterns:** Choosing "eventual" or "strong" consistency as a technology label without naming the invariant; assuming a successful write is immediately visible everywhere.

**Escalate when:** The operation involves money, permissions, inventory, legal records, or irreversible side effects.

**Verify:** Test concurrent reads and writes against the stated invariant, including stale replicas and retry conditions.

## Can every externally visible operation be retried safely?

**Default answer pattern:** Design writes and message handlers to be idempotent with a durable operation or idempotency key.

**Tradeoffs:** Deduplication state and idempotency windows add storage and lifecycle rules; they prevent duplicate charges, emails, records, and tool calls.

**Anti-patterns:** Assuming exactly-once delivery, retrying non-idempotent side effects blindly, or generating a new key on each retry.

**Escalate when:** A duplicate action causes financial, security, compliance, or irreversible harm.

**Verify:** Inject duplicate requests and redelivered messages; prove that the resulting business state and side effects occur once.

## What happens when a message arrives late, out of order, or more than once?

**Default answer pattern:** Treat delivery as at-least-once and unordered unless the transport and application contract prove otherwise.

**Tradeoffs:** Ordering keys, version checks, and sequence tracking make consumers more complex; they protect state from old or conflicting events.

**Anti-patterns:** Updating state from every event unconditionally, relying on producer order across partitions, or discarding failures without a replay path.

**Escalate when:** Event order changes permissions, balances, workflow state, or an external tool action.

**Verify:** Replay duplicated and permuted event sequences and confirm convergence to the intended final state.

## Can we safely change data and schemas while old code is still running?

**Default answer pattern:** Make changes backward-compatible first, deploy readers before writers when needed, migrate gradually, and remove old paths only after evidence.

**Tradeoffs:** Expand-contract migrations take more releases but avoid coordinated deployment and rollback hazards.

**Anti-patterns:** Renaming or deleting fields in one deployment, destructive migrations without a restore plan, and backfills that overload production systems.

**Escalate when:** The change rewrites large datasets, affects retention or personal data, or cannot be reversed.

**Verify:** Test mixed-version reads and writes, migration retries, rollback, and a sampled integrity check after backfill.

## How will the system detect, contain, and recover from partial failure?

**Default answer pattern:** Set timeouts, bounded retries with jitter, backpressure or rate limits, durable failure handling, and signals tied to user impact.

**Tradeoffs:** Aggressive timeouts and limits can reject valid work; unlimited retries and queues turn a local failure into a cascade.

**Anti-patterns:** Infinite retries, no timeout, treating a timeout as proof that nothing happened, or dead-letter queues with no owner or replay process.

**Escalate when:** Recovery requires manual repair, can create duplicate side effects, or crosses a service or vendor boundary.

**Verify:** Simulate timeouts, dependency failure, saturated queues, and restart during processing; confirm bounded load and a documented recovery path.
