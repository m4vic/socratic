# Operations and resilience: sources

**Primary sources:**

- Michael T. Nygard, *Release It! Design and Deploy Production-Ready Software*
  — stability patterns and antipatterns: timeouts, circuit breakers,
  bulkheads, fail fast, and the integration points where systems actually
  come apart.
- Betsy Beyer et al., *Site Reliability Engineering* and *The Site Reliability
  Workbook* (Google) — error budgets, symptom-based alerting, load shedding,
  graceful degradation, and release engineering.

**Supporting material:**

- Sidney Dekker, *The Field Guide to Understanding Human Error* — why
  alert fatigue and on-call load are design problems rather than
  discipline problems.
- Martin Kleppmann, *Designing Data-Intensive Applications* — already the
  source for the `data-systems` pack. That pack covers correctness of state
  under failure; this one covers keeping the service answering while failure
  is happening.

The two packs overlap at retries and queues and are usefully loaded together
for anything with durable state under load.

A future `full.md` could add decision clusters for circuit breaker tuning,
capacity planning and headroom, incident command and escalation paths,
error budget policy, and dependency failure matrices. Add a card only when it
changes a real design choice or verification step.
