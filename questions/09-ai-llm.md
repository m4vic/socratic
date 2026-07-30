# 09 — AI / LLM / Agent Systems

**Load when:** the system calls a model, uses prompts, does retrieval, runs an agent loop, or exposes tools to a model.

## Priority 1

1. Does this actually need an LLM, or would rules, search, or a small classifier be cheaper and more reliable?
2. What's the failure cost of a wrong answer — mildly annoying, or someone loses money?
3. Is a human reviewing output before it has effect, or does the model act autonomously?
4. What's the expected volume, and what does that cost per month at list price?
5. Which model, and what's the fallback if it's unavailable or deprecated?

## Model selection

6. Do you need the frontier model, or does a smaller/cheaper one pass your evals?
7. Is latency a user-facing constraint, and does that rule out the slowest models?
8. Is streaming needed for perceived responsiveness?
9. Is there a reason this must run locally or on-prem (data residency, offline, cost)?
10. Is the model version pinned, and what breaks when the provider updates it?
11. Is there a second provider configured, and has failover been tested?

## Prompt design

12. Is the prompt stored in version control, or embedded in a string somewhere?
13. Is there a clear separation between system instructions, retrieved context, and user input?
14. Is user input clearly delimited so it can't be confused with instructions?
15. Are there few-shot examples, and do they cover the failure cases rather than only the easy ones?
16. Is the output format specified, and enforced by schema/structured output rather than hope?
17. What happens if the model returns malformed output — retry, repair, or fail?
18. Is the prompt tested against variations of user phrasing, or only the one you wrote it for?
19. Are prompts templated safely — can user input break out of the template?

## Context & retrieval (RAG)

20. What's the corpus, how big is it, and how often does it change?
21. What's the chunking strategy, and does it split meaning across boundaries?
22. What embedding model, and is the index rebuilt when it changes?
23. Is retrieval evaluated separately from generation — do the right documents actually come back?
24. Is there hybrid search (keyword + vector), or vector only?
25. Is there reranking, and does it measurably help?
26. How many chunks go into context, and what's the token cost of that?
27. What happens when nothing relevant is retrieved — does the model say "I don't know" or invent?
28. Are sources cited back to the user, and are the citations verifiable?
29. Are documents access-controlled per user, or does retrieval leak across tenants?
30. How stale can retrieved content be before it's wrong?

## Agent & tool use

31. What tools does the agent have, and what's the worst thing each one can do?
32. Which tools are read-only, and which have side effects?
33. Do side-effecting tools require confirmation, and from whom?
34. Is there a step limit / loop budget, and what happens when it's hit?
35. Can the agent get stuck retrying the same failing action forever?
36. Are tool inputs validated before execution, treating model output as untrusted?
37. Is there a spending cap per task and per day?
38. Is state persisted between steps, and can a failed run be resumed rather than restarted?
39. Can the user see what the agent is doing while it works?
40. Can the user interrupt or cancel mid-run, and does that leave things consistent?
41. Are the agent's actions logged in a way that supports an audit afterward?
42. If the agent runs on a schedule, who notices when it silently does nothing?

## Safety & robustness

43. Is content from external sources (web pages, documents, emails) treated as untrusted input that may contain injected instructions?
44. Are tool permissions scoped so that even a misled agent has limited reach?
45. Is there output filtering for the categories that matter to your product?
46. What stops the system from confidently asserting things it doesn't know?
47. Is there a confidence signal, or an explicit "I'm not sure" path?
48. Are there topics or requests the system should refuse, and is that behavior tested?
49. Is PII sent to the model provider, and does your agreement with them allow it?
50. Is model input/output retained by the provider, and for how long?

## Evaluation

51. Is there an eval set, or are you testing by vibes?
52. How many examples in the eval set, and do they include the hard cases and past failures?
53. What's the metric — exact match, LLM-as-judge, human rating, task success?
54. Is the eval run automatically when the prompt or model changes?
55. Is there a regression check so an improvement for case A doesn't break case B?
56. Is there a baseline (previous version, simpler approach) to compare against?
57. How is real production quality measured — thumbs, task completion, escalation rate?
58. Is there a way to collect and review real failures from production?

## Cost & performance

59. What's the average and worst-case token count per request?
60. Is prompt caching applicable, and is the prompt structured to benefit from it?
61. Are you sending more context than the task needs?
62. Is there a cheaper model for the easy majority of requests, with escalation for the hard ones?
63. Are there per-user rate limits, so one user can't run up the bill?
64. Is retry logic multiplying cost during an outage?
65. What's the p95 latency, and does the UI cover it gracefully?

## User experience

66. Does the user know they're interacting with AI?
67. Is uncertainty communicated, or does everything read as equally confident?
68. Can the user correct or override the output?
69. Is there a feedback mechanism, and does anyone look at it?
70. What's shown when the model fails entirely — a useful fallback or a stack trace?

## Verification

- Run the full eval set and compare against the previous version.
- Feed adversarial and off-topic inputs and check the system stays in scope.
- Feed a document containing instruction-like text and confirm the agent doesn't follow it.
- Ask a question the corpus can't answer and confirm the system says so.
- Check retrieval quality independently: for 20 queries, is the right doc in the top-k?
- Measure real token cost over a realistic sample, not a single call.
- Test with the model provider returning 429 and 500.
- Confirm one user cannot retrieve another user's documents.
