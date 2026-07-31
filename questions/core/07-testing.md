# Core Testing and Quality

1. What failure would be most expensive to ship?
2. Which behaviors need unit, contract, integration, and end-to-end coverage?
3. Are boundary, malformed, denied, timeout, and dependency-error paths covered?
4. Are tests isolated, deterministic, fast, and representative of real dependencies?
5. Which lint, type, security, and dependency checks must block integration?
6. What can only be verified in a real environment or by a human?

Verify that tests fail when core behavior is broken, run repeatedly without flakes, and work without hidden local state.

