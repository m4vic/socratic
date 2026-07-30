# 07 — Testing & Quality

**Load when:** the code will be maintained, changed, or relied on by anyone other than the author today.

## Priority 1

1. What's the minimum test coverage that makes this safe to change in three months?
2. What's the one bug in this system that would be most expensive to ship?
3. Is there an existing test setup to follow, or does one need to be created?
4. Is the code structured so it can be tested without spinning up the whole system?
5. Do tests run in CI, and does a failure block merge?

## Test strategy

6. What's tested by unit tests versus integration versus end-to-end?
7. Are you testing behavior or implementation details? (The latter breaks on every refactor.)
8. Which parts genuinely need tests, and which are trivial glue not worth it?
9. Are the expensive end-to-end tests limited to critical paths only?
10. Is there a smoke test that runs against production after deploy?
11. Are contract tests needed between services?

## Test quality

12. Does each test have a clear name saying what behavior it protects?
13. Do tests fail with a message that tells you what's wrong without opening the file?
14. Are tests independent, or do they depend on execution order?
15. Is there shared mutable state between tests?
16. Are there flaky tests, and are they fixed or retried-until-green?
17. Do tests use fixed clocks and seeded randomness, or do they depend on `now()` and luck?
18. Are tests fast enough that developers run them locally?

## Coverage of cases

19. What are the boundary values — zero, one, empty, max, negative, null?
20. What happens with unicode, emoji, very long strings, and RTL text in text fields?
21. Are error paths tested, or only the happy path?
22. Are permission checks covered by tests for the *denied* case?
23. Are concurrency scenarios tested at all?
24. Are timezone and DST edge cases relevant, and covered?
25. Are there tests for the exact bug that was reported, so it can't regress?

## Test data & doubles

26. Where does test data come from — factories, fixtures, or a dumped database?
27. Are external services mocked, stubbed, or hit for real?
28. Do the mocks match the real service's actual behavior, including its errors?
29. Is there a way to test against the real dependency periodically to catch drift?
30. Is test data isolated per test run, so parallel runs don't collide?

## Manual & exploratory

31. What can only be checked by a human looking at it?
32. Is there a manual QA checklist for release?
33. Has anyone tried to use this the wrong way on purpose?
34. Has it been tested on the actual devices/browsers users have, not just yours?

## Static analysis

35. Is there a linter and formatter, and are they enforced rather than suggested?
36. Is there static typing, and are there escape hatches (`any`, `# type: ignore`) accumulating?
37. Is there a security or dependency scanner in CI?
38. Are compiler/interpreter warnings treated as errors?

## Verification

- Delete a line of core logic and confirm a test fails. If nothing fails, the tests aren't testing.
- Run the suite three times and check for flakes.
- Run the suite on a clean machine with no local state.
- Check that the denied-permission cases have tests, not just the allowed ones.
- Time the suite; if it's over the threshold where people skip it, fix that.
