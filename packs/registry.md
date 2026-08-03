# Pack registry

Read this only after choosing the relevant base domains and Core/Full depth. Select a pack by its decision need, not by a book name. Load zero to two packs.

| Pack | Use when the task needs help with | Usually pair with |
|---|---|---|
| `software-design` | complexity, module boundaries, interfaces, abstraction depth, naming, or accidental generality | Requirements, Backend, Testing, Team/Maintenance |
| `domain-modeling` | carving a system into boundaries, naming concepts, deciding what must be consistent together, or one word meaning several things | Requirements, Backend, Data, API, Team/Maintenance |
| `data-systems` | durable state, data ownership, consistency, queues, retries, caching, migrations, replication, or recovery | Data, Backend, Infra, Observability, Testing |
| `operations` | timeouts, retries, load shedding, backpressure, rollback, alerting, or anything that must stay up under failure | Infra, Observability, Backend, Cost/Performance, Testing |
| `threat-modeling` | trust boundaries, attacker paths, abuse cases, privilege escalation, data exposure, or security mitigations | Security, API, Data, Infra, Testing |
| `ai-engineering` | LLM products, RAG, model selection, evals, prompt/version changes, tool use, or model serving cost | AI/LLM, Security, Cost/Performance, Observability, Testing |
| `agent-design` | building an agent or subagent, splitting responsibilities across agents, tool permissions, model tiering, or verifying agent output | AI/LLM, Security, Testing, Product/UX |
| `legacy-change` | modifying code that already works, has no tests, or nobody fully understands; incremental replacement | Testing, Backend, Team/Maintenance, Infra |
| `testing-design` | deciding what to test, what to mock, or why a suite is slow, brittle, or untrusted | Testing, Backend, Data, Team/Maintenance |
| `product-discovery` | whether the thing should exist at all — unproven value, unvalidated problem, undefined audience | Requirements, Product/UX, Cost/Performance |

## Choosing between adjacent packs

- `software-design` covers module depth and interface cost; `domain-modeling` covers where the boundaries should fall in the first place.
- `data-systems` covers correctness of state under failure; `operations` covers staying available while failure happens. Load both for durable state under load.
- `ai-engineering` covers the model layer; `agent-design` covers the orchestration above it.
- The Testing domain establishes what must be covered; `testing-design` decides whether the resulting tests are worth keeping.
- `product-discovery` runs before the engineering packs, not alongside them.

## Source provenance

| Pack | Primary source |
|---|---|
| `software-design` | *A Philosophy of Software Design* by John Ousterhout |
| `domain-modeling` | *Domain-Driven Design* by Eric Evans; *Implementing Domain-Driven Design* by Vaughn Vernon |
| `data-systems` | *Designing Data-Intensive Applications* by Martin Kleppmann |
| `operations` | *Release It!* by Michael T. Nygard; *Site Reliability Engineering* (Google) |
| `threat-modeling` | *Threat Modeling: Designing for Security* by Adam Shostack; *Security Engineering* by Ross Anderson |
| `ai-engineering` | *AI Engineering* by Chip Huyen |
| `agent-design` | Structure observed across 34 agents shipped in first-party Claude Code plugins; Anthropic agent and skill authoring guidance |
| `legacy-change` | *Working Effectively with Legacy Code* by Michael C. Feathers; *Refactoring* by Martin Fowler |
| `testing-design` | *Unit Testing: Principles, Practices, and Patterns* by Vladimir Khorikov |
| `product-discovery` | *The Mom Test* by Rob Fitzpatrick; *Inspired* by Marty Cagan |

Source material informs the decision cards. It is not loaded in full, copied verbatim, or treated as an authority above the task's actual constraints.

`agent-design` is deliberately empirical rather than book-derived: structure observed in shipped systems beats structure argued from first principles.
