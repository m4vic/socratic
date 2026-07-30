# 04 — API Design

**Load when:** designing HTTP/REST/GraphQL/gRPC APIs, SDKs, webhooks, or any contract other code depends on.

## Priority 1

1. Who consumes this — your own frontend, internal services, or external third parties? (Public APIs are forever.)
2. REST, GraphQL, gRPC, or RPC-over-HTTP — and what makes that the right fit?
3. Is this contract going to change, and is there a versioning strategy before v1 ships?
4. Is it authenticated, and by what mechanism?
5. Is there an existing API in this system whose conventions this must match?

## Resource & endpoint design

6. What are the resources, and do the URLs read as nouns?
7. Are HTTP verbs used correctly — is anything destructive behind a GET?
8. Are bulk operations needed, or will clients loop over single calls?
9. Is filtering, sorting, and pagination supported consistently across list endpoints?
10. What's the default page size, and what's the maximum a client can request?
11. Are partial responses / field selection needed, or is that premature?
12. Are there endpoints that return wildly different shapes based on a parameter?

## Request & response contract

13. Is there a schema definition (OpenAPI, protobuf, JSON Schema) generated or hand-written?
14. Are field names consistent in casing across every endpoint?
15. Are timestamps ISO 8601 with timezone? Are enums strings, not magic numbers?
16. Are IDs strings even if they're currently numeric, to allow future change?
17. Is `null` distinguished from "absent" from "empty"?
18. Is there a consistent envelope, or are resources returned bare?
19. Are money, quantities, and units unambiguous in the response?

## Errors

20. Is there one error shape used everywhere?
21. Do errors include a stable machine-readable code, not just a human message?
22. Are validation errors field-level, so clients can attach them to inputs?
23. Are the right status codes used — 400 vs 401 vs 403 vs 404 vs 409 vs 422 vs 429?
24. Do 5xx responses leak stack traces or internal details?
25. Is there a request ID in every response so a user can report a specific failure?

## Versioning & compatibility

26. Where does the version live — URL, header, or content negotiation?
27. What counts as a breaking change in your definition, and is that written down?
28. Can fields be added without breaking clients (are clients required to ignore unknowns)?
29. What's the deprecation policy and notice period?
30. How are consumers notified of changes?

## Rate limiting & abuse

31. Is there a rate limit, per what key, and at what threshold?
32. Are limits communicated via headers, and does 429 include a retry-after?
33. Are expensive endpoints limited more aggressively than cheap ones?
34. Can a single client degrade service for everyone else?
35. Is there a max payload size, max array length, max nesting depth?

## Idempotency & side effects

36. Are POST operations idempotent via an idempotency key?
37. Is it safe for a client to retry a timed-out request?
38. Are long-running operations async with a job/status endpoint rather than a held-open connection?

## Webhooks (if applicable)

39. Are webhook payloads signed, and is signature verification documented?
40. What's the retry schedule for failed deliveries, and for how long?
41. Can events arrive out of order or more than once, and is that documented?
42. Is there a way for consumers to replay missed events?
43. Is there a timeout on the consumer's response, and what happens if they're slow?

## Documentation & DX

44. Is there a working example request for every endpoint?
45. Can someone authenticate and make a first successful call in under five minutes?
46. Are error codes documented with causes and fixes?
47. Is there a sandbox or test mode with fake data?
48. Are there SDKs, and are they generated or maintained by hand?

## Verification

- Call every endpoint with a missing auth token, a wrong token, and another user's resource ID.
- Send an unknown field and confirm it's ignored, not rejected.
- Exceed the rate limit and check the headers and status code.
- Retry an idempotent POST with the same key and confirm one side effect.
- Validate every response against the published schema.
- Have someone unfamiliar follow the docs and make a call cold.
