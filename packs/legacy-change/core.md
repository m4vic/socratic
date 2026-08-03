# Changing existing code: Core pack

Use this pack when modifying code that already works, has no tests, or nobody fully understands. This is most real engineering work, and it follows different rules from building something new.

## Can this be tested before it is changed?

**Default answer pattern:** Get the code under test first, then change it. If testing requires changing it, make the smallest structural change that creates a testing seam — extract a method, introduce a parameter, wrap a dependency — and change nothing else in that step.

**Tradeoffs:** Enabling tests before behavior means two passes instead of one, and the first pass delivers nothing visible.

**Anti-patterns:** Changing behavior and structure in the same commit, so a failing test cannot tell you which broke it. "I will add tests after." Rewriting a class to be testable while also fixing the bug.

**Escalate when:** No seam exists without a change too large to make safely untested. That risk needs stating before starting, not discovering halfway.

**Verify:** Make the structural change and confirm every existing test still passes and the diff contains no logic change.

## Do you know what this code currently does?

**Default answer pattern:** Write tests that pin the *current* behavior, including behavior that looks wrong. Their purpose is to detect change, not to assert correctness. Only once behavior is pinned is it safe to alter it deliberately.

**Tradeoffs:** These tests encode bugs as expected results, which feels wrong and reads badly. They are the only thing that will tell you the rewrite changed something nobody intended.

**Anti-patterns:** Fixing an apparent bug while writing the test — now nothing pins the original behavior. Testing only the paths the current ticket touches. Assuming documented behavior matches actual behavior.

**Escalate when:** Pinned behavior is clearly wrong and something downstream may depend on it. Whether to preserve a bug is a product decision.

**Verify:** Run the pinned tests against the untouched code. Any failure means the tests describe an imagined system, not the real one.

## Refactor in place, or replace?

**Default answer pattern:** Refactor in place by default. Replace incrementally — route a slice of traffic or one feature to the new implementation while the old one keeps serving — and retire the old path only when nothing calls it. Reserve full rewrites for when the platform itself is gone.

**Tradeoffs:** Incremental replacement means both implementations exist for a while, with the routing and duplication that implies. It also means every step is releasable and reversible.

**Anti-patterns:** A rewrite branch that lives for months. "We will catch up on features after the migration." Replacing the parts that are pleasant to rewrite rather than the parts that hurt.

**Escalate when:** A rewrite is being chosen because the existing code is unpleasant rather than because it is blocking. That is a cost the user should approve explicitly.

**Verify:** Name what ships at the end of the first week. If the answer is nothing, the plan is a rewrite regardless of its label.

## How big should one change be?

**Default answer pattern:** One behavior-preserving refactor, or one behavior change, per commit — never both. Keep each step small enough that its correctness is obvious without running anything.

**Tradeoffs:** Many small commits are noisier in history and slower in ceremony. They also make a bisect meaningful and a revert precise.

**Anti-patterns:** A commit titled "refactor and fix". Renaming across a file while changing a condition inside it. Batching unrelated cleanups because the file was already open.

**Escalate when:** A change genuinely cannot be decomposed — usually a data migration. Say so and plan the rollback explicitly.

**Verify:** For each commit, state whether behavior changed. Any commit where the answer is "partly" should be split.

## Where do you cut the dependency?

**Default answer pattern:** Break the dependency at the narrowest point that isolates what you need to test — usually by passing the collaborator in rather than constructing it inside. Prefer the seam that requires the least edit to existing call sites.

**Tradeoffs:** Every seam adds an indirection that future readers must follow. Some are permanent design improvements; others are scaffolding that should be removed once tests exist.

**Anti-patterns:** Introducing an interface with one implementation solely to enable mocking, then leaving it forever. Extracting a class to break a dependency and giving it a name that describes the mechanism rather than the concept.

**Escalate when:** The seam requires changing a published contract. That affects callers you do not control.

**Verify:** Name the concept the new boundary represents. If it has no name in the domain, it is scaffolding — mark it for removal.

## Is this change actually needed here?

**Default answer pattern:** Improve the code you had to touch anyway, in the direction the current change points. Leave the rest alone, however tempting.

**Tradeoffs:** Restraint leaves known bad code in place. It also keeps the diff reviewable and the blast radius bounded, which is what lets the change ship at all.

**Anti-patterns:** Opportunistic reformatting that buries the real change. Fixing an unrelated bug spotted in passing, so a revert now undoes two things. Cleanup with no test coverage as cover.

**Escalate when:** The surrounding code is dangerous enough that leaving it feels negligent. Raise it as separate work with its own justification.

**Verify:** Read the diff as a reviewer. Every hunk should be explainable by the stated purpose of the change.
