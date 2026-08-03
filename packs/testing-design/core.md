# Testing design: Core pack

Use this pack when deciding what to test, what to mock, or why a test suite is slow, brittle, or trusted by nobody. The Testing domain asks whether coverage exists; this pack asks whether the tests are worth keeping.

## What makes this test worth its maintenance cost?

**Default answer pattern:** A test earns its place by catching real regressions, surviving refactoring, running fast, and being readable. A test that fails all four should be deleted rather than fixed — deleting it is a net gain.

**Tradeoffs:** Deleting tests feels like reducing safety and reads badly in coverage reports. A suite nobody trusts provides no safety already.

**Anti-patterns:** Coverage percentage as a goal. Keeping a flaky test "until someone has time". One test per method by convention.

**Escalate when:** Coverage is contractually or legally required. Then the target is externally set and the trade is not yours.

**Verify:** For each test, name the bug it would catch. Tests with no answer are ceremony.

## Will this test survive a refactor?

**Default answer pattern:** Assert on observable outcomes, not on how they were produced. A test that breaks when internals change without behavior changing is a liability — it trains people to update tests reflexively, which destroys their signal.

**Tradeoffs:** Outcome-level tests localize failures less precisely and can require more setup.

**Anti-patterns:** Asserting a private method was called. Verifying call order that the contract does not require. Snapshot tests regenerated whenever they fail.

**Escalate when:** The observable outcome is genuinely unreachable — a fire-and-forget side effect. That is a design problem surfacing as a testing problem.

**Verify:** Rename an internal method and re-run. Any failure is coupling to implementation.

## Should this dependency be mocked?

**Default answer pattern:** Mock only what is outside the application *and* observable to someone else — a third-party API, an email, a message another service consumes. Use the real thing for dependencies only you can see, including your own database.

**Tradeoffs:** Real databases make tests slower and require setup. They also test the query that actually runs, which is where a large share of real bugs live.

**Anti-patterns:** Mocking the repository layer and calling the result an integration test. Mocking your own code. So many mocks the test restates the implementation line by line.

**Escalate when:** A real dependency cannot be used in CI. That is an infrastructure decision with a correctness cost.

**Verify:** Break a SQL query deliberately. If nothing fails, the database is mocked at the wrong level.

## What kind of code is this, and does it need a test at all?

**Default answer pattern:** Test complex code with few dependencies thoroughly. Test simple code with many dependencies at the integration level. Leave trivial code untested. Code that is both complex and heavily dependent should be split before it is tested.

**Tradeoffs:** Splitting to make code testable is a real refactor with real risk, done before the feature.

**Anti-patterns:** Unit-testing a controller that only forwards calls. Elaborate mock setups to reach one branch of business logic buried in a handler. Testing getters.

**Escalate when:** Splitting the code touches a published contract.

**Verify:** Count the branches and the collaborators. High in both means extract the logic first.

## Unit, integration, or end-to-end?

**Default answer pattern:** Put the bulk of behavior in fast tests against real internal collaborators. Add a thin layer of end-to-end tests covering only the paths whose failure would be unacceptable. Balance by risk, not by a fixed ratio.

**Tradeoffs:** End-to-end tests catch wiring failures nothing else can, and they are slow, flaky, and expensive to diagnose.

**Anti-patterns:** An end-to-end test for every user story. A pyramid shape enforced as policy regardless of where the risk sits. No end-to-end tests at all, so nobody knows whether the deployed system starts.

**Escalate when:** End-to-end runtime is long enough that people skip it. That is a delivery problem, not a testing preference.

**Verify:** Break the wiring between two components without breaking either. Something should fail.

## Does this test fail for the right reason?

**Default answer pattern:** Make every test fail once, deliberately, before trusting it. Read the failure message and confirm it names what actually broke.

**Tradeoffs:** Deliberate breakage takes a minute per test and feels redundant when the test already passes.

**Anti-patterns:** Tests that pass because an assertion never ran. Async tests that finish before the assertion. `assertTrue(true)` left after debugging. Try/catch swallowing the failure.

**Escalate when:** A test cannot be made to fail. It is asserting nothing.

**Verify:** Invert the behavior under test and confirm the failure message would let someone diagnose it without reading the test.
