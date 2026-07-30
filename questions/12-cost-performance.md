# 12 — Cost & Performance

**Load when:** scale, latency, or spend is a real constraint — which is most production systems.

## Priority 1

1. What's the latency budget for the primary operation, measured at p95?
2. What's the expected load today, and the load you'd be embarrassed to fall over at?
3. What's the monthly budget, and what's the current run rate?
4. Which resource runs out first — CPU, memory, connections, IOPS, API quota, or money?
5. Is performance currently a problem, or are you optimizing preemptively?

## Measurement first

6. Has this been profiled, or is the bottleneck a guess?
7. What's the baseline you're comparing against?
8. Is the measurement from production-like data, or a toy dataset?
9. Are you measuring p50, p95, and p99 separately? Averages hide the pain.
10. What does the latency breakdown look like by component?

## Latency

11. What's the critical path, and how many sequential network hops are on it?
12. Can independent calls be parallelized?
13. Is anything blocking that could be deferred to after the response?
14. What's the slowest dependency, and is its timeout shorter than your budget?
15. Is there a cold-start penalty, and how often do users hit it?
16. Is perceived latency addressed even where actual latency can't be (streaming, skeletons, optimistic UI)?

## Throughput & concurrency

17. What's the max concurrent requests a single instance can handle?
18. Is there a thread/connection/worker pool, and is it sized correctly?
19. Is there queueing, and what's the queue depth alarm threshold?
20. Does the system apply backpressure, or accept work it can't finish?
21. What's the behavior at 2x, 5x, and 10x expected load?

## Data-layer performance

22. Are the hot queries indexed and verified with a query plan?
23. Is there an N+1 pattern anywhere in the request path?
24. Are large result sets streamed or paginated rather than loaded fully into memory?
25. Is there caching where the same expensive result is computed repeatedly?
26. Are batch operations used instead of loops of single operations?

## Cost drivers

27. What's the unit cost — per request, per user, per GB, per token?
28. Which line item dominates the bill?
29. Are there costs that scale superlinearly with usage?
30. Is data egress charged, and is anything crossing regions unnecessarily?
31. Are third-party API calls metered, and is there a cap?
32. Are there idle resources provisioned for peak that could be autoscaled?
33. Is storage growing without a retention policy?
34. Are logs and metrics themselves a significant cost?

## Efficiency vs. complexity

35. Is the optimization worth the complexity it adds?
36. Would buying a bigger instance be cheaper than the engineering time to optimize?
37. Is there a simpler algorithm or data structure that removes the problem?
38. Is caching being used to paper over a design problem?

## Guardrails

39. Is there a budget alert before the bill becomes a surprise?
40. Is there a per-user or per-key quota so one consumer can't dominate?
41. Is there a kill switch for an expensive feature?
42. Are runaway loops or retries bounded in cost, not just in count?

## Verification

- Load-test at the target rate and at 3x, and record where it breaks.
- Profile the primary path and confirm the bottleneck matches your assumption.
- Run the cost calculation against a week of real usage, not an estimate.
- Check the query plans on the top queries against production-size data.
- Simulate a dependency being slow (not down) and see whether the system queues to death.
