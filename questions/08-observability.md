# 08 — Observability & Operations

**Load when:** anything will run unattended in production.

## Priority 1

1. How will you know this is broken before a user tells you?
2. What's the one metric that says "this system is healthy"?
3. Who gets paged, and for what?
4. When something fails, what's the first place you look?
5. Can you trace a single user's request through the whole system?

## Logging

6. What's logged at each level, and is the level actually meaningful?
7. Are logs structured (JSON) or free text?
8. Is there a request/correlation ID propagated across services?
9. Is enough context logged to debug without reproducing — inputs, IDs, timing?
10. Is anything sensitive being logged? (See `05-security.md`.)
11. What's the log volume, and what does it cost?
12. What's the retention period, and is it long enough for the slowest-noticed bug?
13. Can you search logs by user ID, request ID, and error type?

## Metrics

14. What are the golden signals here — latency, traffic, errors, saturation?
15. Are latency metrics percentiles (p50/p95/p99), not averages?
16. Are business metrics tracked, not just technical ones (signups, orders, jobs completed)?
17. Are there metrics on queue depth, job age, and retry counts?
18. Is there a metric that would go to zero if the system silently stopped working?
19. Are metrics cardinality-safe, or will a user-ID label blow up the metrics store?

## Alerting

20. Which alerts wake someone up, and which just file a ticket?
21. Is every paging alert actionable, with a runbook?
22. What's the false-positive rate, and are people already ignoring alerts?
23. Are there alerts on symptoms (users affected) rather than only causes (CPU high)?
24. Is there an alert for "nothing happened when something should have" — a stalled cron, an empty queue?
25. Is there alert deduplication and escalation?

## Tracing & debugging

26. Are traces sampled, and is the sample rate enough to catch rare failures?
27. Can you correlate a trace to logs to metrics for a single incident?
28. Are slow database queries logged with the query and its parameters?
29. Are errors sent to an error tracker with stack traces and grouping?
30. Is there a way to reproduce production state safely in a debug environment?

## Dashboards

31. Is there a single dashboard someone unfamiliar could open during an incident?
32. Does the dashboard show deploy markers so you can correlate changes to breakage?
33. Are dependency health and third-party status visible?

## Operational readiness

34. Is there a runbook for the top three failure modes?
35. What manual interventions might be needed, and is there a safe tool for them?
36. Is there a way to re-run failed jobs, replay events, or backfill data?
37. Who owns this system, and is that written down somewhere findable?
38. What's the on-call handoff — does the next person know what's currently degraded?
39. Is there a postmortem practice, and are action items tracked?

## Verification

- Trigger a real failure in staging and confirm the alert fires and the runbook works.
- Pick a random request and trace it end to end using only the observability tools.
- Check that a deploy shows up on the dashboard.
- Silence the primary dependency and confirm the alert distinguishes "our bug" from "their outage."
- Ask someone else to debug a seeded issue using only the dashboards and logs.
