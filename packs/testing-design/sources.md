# Testing design: sources

**Primary source:**

- Vladimir Khorikov, *Unit Testing: Principles, Practices, and Patterns* —
  the four pillars of a valuable test (protection against regressions,
  resistance to refactoring, fast feedback, maintainability), the code
  quadrant that decides what deserves a unit test, and the rule that only
  unmanaged out-of-process dependencies should be mocked.

**Supporting material:**

- Steve Freeman and Nat Pryce, *Growing Object-Oriented Software, Guided by
  Tests* — test-driven design pressure and mocking at the right boundary.
- Michael C. Feathers, *Working Effectively with Legacy Code* — already the
  source for the `legacy-change` pack. Characterization tests are the
  entry point when there is no suite to reason about yet, and the two packs
  pair well on untested code.
- Martin Fowler, *TestPyramid* and *Practical Test Pyramid* (Ham Vocke) —
  balancing test levels by risk rather than by fixed ratio.

This pack is depth for the Testing domain, not a replacement: the domain
questions establish what must be covered, and these cards decide whether the
resulting tests are worth keeping.

A future `full.md` could add decision clusters for test data management,
contract testing between services, mutation testing as a coverage check,
property-based testing, and diagnosing flakiness by cause. Add a card only
when it changes a real design choice or verification step.
