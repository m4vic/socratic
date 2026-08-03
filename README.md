<p align="center">
  <img src="assets/pix.png" alt="Socratic Logo" width="380">
</p>

<h1 align="center">Socratic</h1>

<p align="center">
  Meta - questioning about questioning. Question yourself till you're left with only answers.
</p>



**Socratic** is a self-interrogation skill for agentic AI packaging 697 questions a senior engineer asks before writing a single line of code into a portable Claude and Codex skill/prompt.

It helps an LLM slow down, inspect the task, ask itself the right engineering questions, and only ask the user for the few decisions that actually require human authority.

Not a form. Not a checklist. A lightweight reasoning loop.

## The stopping rule: question until the answer is solid

Socratic is not a 697-question checklist and it is not an endless reasoning loop. It asks the smallest set of relevant questions needed to reach a solid, evidence-backed answer.

The agent stops expanding its review when the outcome and scope are clear, consequential assumptions are visible, material risks have a mitigation or verification path, no contradiction changes the plan, and the riskiest assumption can be tested proportionately. If the next question would not change the design, risk, cost, authority decision, or verification plan, it is no longer material.

This is the purpose of self-interrogation: let an agent challenge its own plan from several useful directions, satisfy the meaningful uncertainty, and then build. It should never keep thinking merely to consume tokens, nor stop only because a token budget is low.

## How it actually works

**The agent interviews itself, not you.** Point it at a task, and it silently works through the relevant slice of the question bank — reading the codebase where it can, applying sensible engineering defaults where it can't, and only stopping to ask you about the handful of decisions that are genuinely yours to make (budget, vendor, legal risk, an irreversible call).

What you see is a short contract before any code is written:

```text
Domains considered:      requirements, data, security, testing, AI/LLM
Self-answered:           Postgres over SQLite (existing driver, concurrent writers)
                         at-least-once delivery; duplicate work is safe here
                         no PII in the queue, so no encryption-at-rest work
Assumed (flag if wrong): single-region; retries capped at 5
Open questions for you:  1. Is a 30-second worst-case job latency acceptable?
Top risks:               retry storm on downstream outage — no circuit breaker yet
Plan:                    durable enqueue → worker drain → backoff on failure
```

One question instead of twelve. The other eleven were answered from your repo.

1. classify the task
2. detect the relevant domains
3. load the smallest useful question set
4. self-interrogate silently
5. surface assumptions, risks, and only necessary open questions
6. build
7. verify


## Core and Full modes
The full question bank contains 697 questions across engineering domains.
That is useful for production-grade, high-risk, or audit-style work, but too expensive to load by default for many agentic workflows.
So Socratic now has two depths:

### Core
Core is the default path.
It loads the highest-signal subset of questions first: roughly the top 90 questions that matter most across routine, prototype, internal, and moderately scoped work.
Use Core for:
- prototypes
- internal tools
- one-off builds
- medium-scope engineering work
- most day-to-day agentic tasks
### Full
Full loads the complete domain files.
Use Full for:
- production systems
- public APIs
- authentication
- payments
- PII or regulated data
- autonomous tools
- costly or irreversible actions
- deep audits
- explicit high-risk reviews


### Why this split matters
The point is not to ask more questions.
The point is to ask the most useful questions at the right time without burning unnecessary context.

### What it actually costs

The whole repository is roughly 29,000 tokens. A normal run loads under a tenth of it. Counted with `tiktoken` (`o200k_base`), not estimated:

| What loads | Tokens | When |
|---|---:|---|
| `SKILL.md` | ~2,300 | Every run |
| **Typical pass** — SKILL + 4 core domains | **~2,800** | Most work |
| All 15 core domains | ~4,100 | Broad prototype review |
| Heavy pass — SKILL + 8 full domains + 1 pack | ~10,900 | Production, money, PII |
| Entire repository | ~29,000 | Never |

For scale: the median `SKILL.md` across Anthropic's 31 officially published skills is **2,255 tokens**. Socratic's entry point is the same size as a first-party skill — the depth lives in files that stay on disk until the task calls for them.

**The Core/Full split is worth 7.5×.** All fifteen compact domain files together are 1,804 tokens; the fifteen complete ones are 13,599. Core files average about 120 tokens each, so even a maximal compact pass costs less than the skill's own instructions.
   
If you'd rather be interviewed live, ask for it ("interview me," "ask me one at a time") and it switches to a one-yes/no-question-per-turn mode instead. That's opt-in, not the default.

## Knowledge-backed packs

`questions/` and `packs/` have different jobs:

- **Questions** provide coverage: they identify the concerns a task must resolve across requirements, security, data, testing, and other domains.
- **Packs** provide depth: they add compact, source-backed reasoning for a specialist decision area after the relevant questions have been selected.

Neither replaces the other. A great book does not cover every product, security, testing, or operational concern; a broad question bank does not contain every hard-won systems-design tradeoff.

The starter structure includes:

- [`packs/registry.md`](packs/registry.md) for deterministic pack selection
- [`packs/software-design/core.md`](packs/software-design/core.md) for complexity, interfaces, and accidental generality
- [`packs/data-systems/core.md`](packs/data-systems/core.md) for distributed data, reliability, and change-management tradeoffs
- [`packs/threat-modeling/core.md`](packs/threat-modeling/core.md) for trust boundaries, abuse paths, and security mitigations
- [`packs/ai-engineering/core.md`](packs/ai-engineering/core.md) for LLM evaluation, reliability, retrieval, tools, cost, and versioning
- [`packs/agent-design/core.md`](packs/agent-design/core.md) for agent boundaries, tool permissions, model tiering, and verifying agent output
- [`packs/operations/core.md`](packs/operations/core.md) for timeouts, retries, load shedding, rollback, and alerting

For each task, the agent first selects the base domains and Core/Full depth, then reads the compact pack registry and adds zero to two relevant packs only where they sharpen the decision. Pack names describe the capability they add; their book sources are documented as provenance.

`agent-design` is the one pack that isn't book-derived. Its decision cards come from structure observed across 34 agents shipped in first-party Claude Code plugins, where seven archetypes recur and the agent that does the work is never the one that verifies it. Structure observed in production beats structure argued from first principles.

## Book-derived knowledge, compressed for agents

Socratic does not copy books into an agent context. It curates reusable decision patterns into short cards: what to ask, the default answer, tradeoffs, common mistakes, escalation conditions, and how to verify the decision. This gives an agent a practical form of self-interrogation: it can question its plan with accumulated engineering knowledge before it builds.

## What's in it

| Domain | Questions | File |
|---|---|---|
| Requirements & scope | 40 | [`questions/00-requirements.md`](questions/00-requirements.md) |
| Frontend & UI | 46 | [`questions/01-frontend.md`](questions/01-frontend.md) |
| Backend & services | 45 | [`questions/02-backend.md`](questions/02-backend.md) |
| Data & storage | 53 | [`questions/03-data.md`](questions/03-data.md) |
| API design | 48 | [`questions/04-api.md`](questions/04-api.md) |
| Security | 59 | [`questions/05-security.md`](questions/05-security.md) |
| Infrastructure & DevOps | 46 | [`questions/06-infra.md`](questions/06-infra.md) |
| Testing & quality | 38 | [`questions/07-testing.md`](questions/07-testing.md) |
| Observability & ops | 39 | [`questions/08-observability.md`](questions/08-observability.md) |
| AI / LLM / agents | 70 | [`questions/09-ai-llm.md`](questions/09-ai-llm.md) |
| Mobile & offline | 41 | [`questions/10-mobile.md`](questions/10-mobile.md) |
| Product & UX | 45 | [`questions/11-product-ux.md`](questions/11-product-ux.md) |
| Cost & performance | 42 | [`questions/12-cost-performance.md`](questions/12-cost-performance.md) |
| Compliance & legal | 42 | [`questions/13-compliance.md`](questions/13-compliance.md) |
| Team & maintenance | 43 | [`questions/14-team-maintenance.md`](questions/14-team-maintenance.md) |

Every file follows the same shape: **Priority 1** questions first, then thematic sections, then a **Verification** block to run *after* the build.

## Dynamic domain selection

The domain set isn't chosen once from the initial request — it's built by scanning for signals and can grow mid-build. Requirements and Testing are always in. Everything else gets pulled in when it matches:

| Signal | Domains added |
|---|---|
| UI, dashboard, form | Frontend |
| service, job, queue | Backend |
| database, schema, cache | Data |
| API, SDK, webhook, **connector**, integration | API |
| auth, payments, secrets, public-facing | Security |
| deploy, CI/CD, cloud, scaling | Infra |
| production, cron, monitoring | Observability |
| AI, LLM, agent, prompt, RAG | AI/LLM |
| mobile, iOS, Android, offline | Mobile |
| anything user-facing | Product/UX |
| scale, latency, high traffic | Cost/Performance |
| personal data, health, EU/CA users | Compliance |
| long-lived, team project | Team/Maintenance |

A "tool with connectors" isn't just an API question — it pulls in API (contract, third-party auth), Security (credential storage per connector, blast radius if one leaks), and Testing (mocking each connector's failure modes) together. If self-answering later reveals a new need — say, a persistent store you didn't expect — the scan re-runs and adds Data mid-task.

## Three ways to use it

### 1. As a Claude Code skill

```bash
mkdir -p ~/.claude/skills
cp -r socratic ~/.claude/skills/
```

### 2. As a Codex skill

Install under `$CODEX_HOME/skills`; when `CODEX_HOME` is unset, use `~/.codex/skills`:

```bash
mkdir -p ~/.codex/skills
cp -r socratic ~/.codex/skills/
```

Invoke explicitly with `$socratic`, or let it trigger implicitly when your request matches.

### 3. As a portable prompt

Paste [`PROMPT.md`](PROMPT.md) into the system prompt of any LLM — ChatGPT, Gemini, a local model, your own agent framework. Self-contained, no file dependencies.

For an always-on prompt with minimal overhead, use [`PROMPT_LITE.md`](PROMPT_LITE.md).

## The self-interrogation loop

```
Scan request + codebase → build working domain set (dynamic, can grow mid-build)
   ↓
Self-answer every question: read codebase → apply engineering default → escalate only if it's a business decision
   ↓
Emit contract once: domains / highlights / assumed / open questions (0-3 ideally) / risks / plan
   ↓
Ask the (few) open questions, if any — batched
   ↓
Build
   ↓
Run Verification for every domain in the final set, including ones added mid-build
```

## Design principles

- **Self-answer by default.** The bank exists so the agent has more engineering perspective, not so the user fills out a form.
- **Read before assuming; assume before asking.** Escalation to the user is the last resort, reserved for decisions only they can authorize.
- **Domain set is dynamic.** It's built from signals in the request and code, and can grow as the agent learns more mid-task — not fixed at the first guess.
- **Testing runs every time.** Not gated behind the user asking for it — any tool/service/script that gets built gets its testing questions and verification pass.
- **Keep "Open questions" near zero.** A long list of open questions means engineering decisions got escalated that shouldn't have been.
- **Interactive mode is opt-in**, for when a user explicitly wants to be walked through it live.

## Extending it

Add a domain by dropping `questions/15-yourdomain.md` in, following the existing shape (Priority 1 → sections → Verification), then add a row to the signal table in `SKILL.md` and `PROMPT.md` so it gets picked up dynamically.

For source-backed specialist knowledge, add a pack under `packs/<name>/core.md` and optionally `packs/<name>/full.md`. Keep packs compact and decision-shaped rather than writing long summaries.

## License

MIT — see [LICENSE](LICENSE). Use it, fork it, ship it.

## Contributing

PRs welcome, especially:

- Domains not covered (embedded, games, blockchain, hardware, accessibility-in-depth)
- Questions that came from a real incident — those are the good ones
- Better signal words for dynamic domain detection
