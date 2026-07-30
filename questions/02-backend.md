# 02 — Backend & Services

**Load when:** building services, business logic, background jobs, queues, schedulers, or anything server-side.

## Priority 1

1. Monolith, modular monolith, or separate service? What justifies the split if you're splitting?
2. Synchronous request/response, or asynchronous job/queue? What drives that?
3. What's the expected request rate and payload size?
4. Is this stateless, or does it hold state between requests?
5. What's the runtime and hosting model — long-running server, serverless function, container, cron?

## Business logic

6. Where does the business logic live — in the handler, a service layer, or the model?
7. Are there invariants that must always hold, and where are they enforced?
8. Is the same rule enforced in more than one place, and will they drift apart?
9. Are there workflows with multiple steps that must all succeed or all roll back?
10. Are there state machines, and are illegal transitions actually prevented?
11. Is any logic duplicated between frontend and backend, and which one is authoritative?

## Failure handling

12. What happens when a downstream dependency is slow — do you have a timeout on every outbound call?
13. What happens when a dependency is completely down — fail, degrade, or queue?
14. Is this operation idempotent? If a client retries, does anything get duplicated?
15. What's the retry policy — how many, what backoff, and is there jitter?
16. Is there a circuit breaker for repeatedly failing dependencies?
17. What happens if the process dies halfway through a multi-step operation?
18. Are partial failures detectable, or do they fail silently?
19. Is there a dead-letter queue or failure log for jobs that exhaust retries?
20. Can a failed operation be safely replayed by an operator?

## Concurrency

21. What happens if two requests modify the same record simultaneously?
22. Is optimistic or pessimistic locking appropriate here?
23. Are there race conditions between reading a value and acting on it?
24. Are background jobs safe to run more than once concurrently?
25. Is there a distributed lock needed, and what happens if the lock holder dies?
26. Are there ordering guarantees required on queued messages?

## Background work

27. What runs in-request versus deferred to a job?
28. What's the queue backend, and what happens if it fills up?
29. How does a user find out a background job finished or failed?
30. Are scheduled jobs safe if they overlap or if one run is skipped?
31. Is there a job that would be catastrophic to run twice?

## Input & contracts

32. Is every inbound payload validated against a schema before it touches logic?
33. What's the max request size, and is it enforced?
34. How are validation errors returned — shape, status code, field-level detail?
35. Are internal service-to-service calls validated, or trusted?

## Configuration

36. What's configurable via environment, and what's hardcoded?
37. Does the service fail fast at startup if required config is missing?
38. Are there feature flags, and who can flip them?
39. Are there different behaviors per environment, and is that surface minimal?

## Scale & resources

40. What's the connection pool size, and does it match the database's limits?
41. Are there unbounded in-memory structures that grow with load?
42. Is there anything O(n²) hiding in a loop over user-supplied data?
43. Can this scale horizontally, or is there sticky state preventing it?
44. What's the memory ceiling per instance, and what happens when it's hit?
45. Is there backpressure, or does the service accept work faster than it can process it?

## Verification

- Kill a dependency and confirm the service degrades the way you claimed.
- Send the same request twice and confirm no duplicate side effects.
- Send a malformed, oversized, and empty payload to every endpoint.
- Run two concurrent writes to the same record and check the outcome.
- Start the service with a required env var missing — does it fail loudly?
- Load-test at the Q3 rate and watch memory, connections, and latency.
