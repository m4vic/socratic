# Pack registry

Read this only after choosing the relevant base domains and Core/Full depth. Select a pack by its decision need, not by a book name. Load zero to two packs.

| Pack | Use when the task needs help with | Usually pair with |
|---|---|---|
| `software-design` | complexity, module boundaries, interfaces, abstraction depth, naming, or accidental generality | Requirements, Backend, Testing, Team/Maintenance |
| `data-systems` | durable state, data ownership, consistency, queues, retries, caching, migrations, replication, or recovery | Data, Backend, Infra, Observability, Testing |
| `threat-modeling` | trust boundaries, attacker paths, abuse cases, privilege escalation, data exposure, or security mitigations | Security, API, Data, Infra, Testing |
| `ai-engineering` | LLM products, RAG, model selection, evals, prompt/version changes, tool use, or model serving cost | AI/LLM, Security, Cost/Performance, Observability, Testing |
| `agent-design` | building an agent or subagent, splitting responsibilities across agents, tool permissions, model tiering, or verifying agent output | AI/LLM, Security, Testing, Product/UX |
| `operations` | timeouts, retries, load shedding, backpressure, rollback, alerting, or anything that must stay up under failure | Infra, Observability, Backend, Cost/Performance, Testing |

`data-systems` and `operations` overlap at retries and queues, and are usefully loaded together for durable state under load. `ai-engineering` covers the model layer; `agent-design` covers the orchestration above it.

## Source provenance

| Pack | Primary source |
|---|---|
| `software-design` | *A Philosophy of Software Design* by John Ousterhout |
| `data-systems` | *Designing Data-Intensive Applications* by Martin Kleppmann |
| `threat-modeling` | *Threat Modeling: Designing for Security* by Adam Shostack; *Security Engineering* by Ross Anderson |
| `ai-engineering` | *AI Engineering* by Chip Huyen |
| `agent-design` | Structure observed across 34 agents shipped in first-party Claude Code plugins; Anthropic agent and skill authoring guidance |
| `operations` | *Release It!* by Michael T. Nygard; *Site Reliability Engineering* (Google) |

Source material informs the decision cards. It is not loaded in full, copied verbatim, or treated as an authority above the task's actual constraints.

`agent-design` is deliberately empirical rather than book-derived: structure observed in shipped systems beats structure argued from first principles.
