# 06 — Infrastructure & DevOps

**Load when:** deployment, CI/CD, containers, cloud resources, networking, scaling, or environment setup are in scope.

## Priority 1

1. Where does this run — a VM, containers, serverless, someone's laptop?
2. How does code get from a commit to running in production?
3. How many environments exist, and how close is staging to production?
4. Who can deploy, and is it a button or a manual sequence of commands?
5. How do you roll back, and how fast?

## Build & CI

6. What runs on every pull request — tests, lint, type check, build?
7. How long does CI take, and is that fast enough that people don't skip it?
8. Are builds reproducible — same commit, same artifact?
9. Are dependencies cached, and is the cache keyed correctly?
10. Does CI run against the same OS/runtime version as production?
11. Are build artifacts versioned and stored, or rebuilt on every deploy?
12. Can CI be run locally to debug a failure?

## Deployment

13. Is the deploy zero-downtime — rolling, blue-green, or does it just restart?
14. Are database migrations run before, during, or after the code deploy, and is the ordering safe?
15. Is there a health check, and does the orchestrator actually wait for it?
16. Is there a canary or staged rollout for risky changes?
17. What's the deploy frequency you're designing for — daily or quarterly?
18. Are deploys reversible, or do migrations make them one-way?
19. Is there a maintenance mode, and does it degrade gracefully?

## Configuration & environments

20. Is configuration separated from code, per twelve-factor?
21. How do secrets reach the runtime?
22. Is there config drift between environments, and how would you detect it?
23. Is infrastructure defined as code, or clicked in a console?
24. If the infra state file were lost, could you rebuild?
25. Can a developer spin up a working local environment in one command?

## Networking

26. What's publicly reachable, and what should be private?
27. Are internal services behind a VPC / private network?
28. Is there a load balancer, and what's its health check and timeout?
29. Are there firewall rules or security groups, and are any of them `0.0.0.0/0`?
30. Is DNS managed as code, and what's the TTL on records you might need to change fast?
31. Are there egress restrictions, or can any compromised container call anywhere?

## Scaling & capacity

32. Does this autoscale, on what metric, and what are the min/max bounds?
33. How long does a new instance take to become useful (cold start, warmup)?
34. What's the bottleneck that scaling won't fix — usually the database?
35. Are there resource limits and requests set on containers?
36. What happens at 10x traffic — degrade, queue, or fall over?
37. Are there quotas or limits on the cloud account that would be hit first?

## Reliability

38. What's the single point of failure in this architecture?
39. Is it multi-AZ, multi-region, or single-box, and is that a deliberate choice?
40. What's the target uptime, and does the architecture plausibly support it?
41. Is there a disaster recovery plan, and has it been rehearsed?
42. What manual step exists that would block recovery at 3am?

## Cost & hygiene

43. What's the monthly cost of this infrastructure, roughly?
44. Are there resources that scale with usage in a way that could surprise you?
45. Are old artifacts, logs, snapshots, and orphaned volumes cleaned up?
46. Are non-production environments shut down outside working hours?

## Verification

- Deploy to staging, then roll back, and time both.
- Kill an instance under load and confirm traffic reroutes.
- Deploy a change that fails its health check — does it get promoted anyway?
- Rebuild the environment from infra-as-code into a scratch account/namespace.
- Review firewall rules for anything open to the world that shouldn't be.
- Check the cloud bill breakdown against expectations.
