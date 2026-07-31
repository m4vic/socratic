<p align="center">
  <img src="assets/pix.png" alt="Socratic Logo" width="180">
</p>

<h1 align="center">Socratic</h1>

<p align="center">
  Meta — questioning about questioning. Question yourself till you're left with only answers.
</p>

<p align="center">
  A self-interrogation skill for agentic AI systems.
</p>

Socratic is a curated engineering question system packaged as a Codex/Claude skill and a portable prompt.

It helps an AI system slow down, inspect the task, ask itself the right questions, surface risks, make reasonable engineering defaults, and only ask the human for the few decisions that actually require human authority.

This is not a form for the user to fill out.

It is a lightweight self-questioning loop for task completion.

## What it does

Instead of immediately generating code from a vague request, Socratic makes the model do this first:

1. Classify the task.
2. Select the relevant engineering domains.
3. Load the smallest useful question set.
4. Ask itself the important questions silently.
5. Read the codebase and available context before assuming.
6. Apply engineering defaults where possible.
7. Escalate only the decisions the user must own.
8. Build.
9. Verify.

That means the model interviews itself, not you.

## Why it exists

Most agentic systems fail for a simple reason: they start executing too early.

They miss requirements, security implications, edge cases, operational constraints, testing gaps, and hidden trade-offs. Socratic adds a structured internal loop before execution so the model can reason more like a senior engineer and less like autocomplete.

You can think of it as a compact reasoning layer for:

- task clarification
- domain-aware planning
- self-interrogation
- risk detection
- assumption control
- verification gating

## Core idea

Socratic turns a static prompt into a task-adaptive loop.

```text
Task arrives
  ↓
Scan request and project
  ↓
Choose relevant domains
  ↓
Load Core or Full depth
  ↓
Self-interrogate silently
  ↓
Surface assumptions, risks, and only the necessary open questions
  ↓
Execute
  ↓
Verify
```

This is why Socratic is more than a list of questions. The questions are the mechanism that powers the loop.

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

## How it actually behaves

By default, Socratic does silent self-interrogation.

It reads the request, inspects the codebase, selects the relevant domains, answers what it can internally, and only asks the user about decisions that are genuinely theirs to make, such as:

- budget
- vendor choice
- legal risk tolerance
- product priority
- irreversible business decisions

If the user wants to be interviewed live, they can opt in with prompts like:

- "interview me"
- "ask one at a time"
- "walk me through the decisions"

Otherwise, Socratic stays out of the way and focuses on helping the model complete the task.

## Question domains

The full bank currently covers 15 domains:

| Domain | Questions | File |
|---|---:|---|
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

Each file follows the same shape:

- Priority 1 questions
- themed sections
- Verification block

The `questions/core/` folder contains the compact high-signal versions used for default operation.

## Dynamic domain selection

Socratic does not load everything.

It builds the working domain set dynamically based on the request and the codebase.

Requirements and Testing are always included. Other domains are added only when they are relevant.

| Signal | Domain |
|---|---|
| UI, page, component, dashboard, form, frontend | Frontend |
| service, endpoint, job, queue, backend, business logic | Backend |
| database, schema, storage, migration, cache | Data |
| API, SDK, webhook, connector, integration, OAuth | API |
| auth, accounts, payments, secrets, public exposure | Security |
| deployment, CI/CD, containers, cloud, scaling | Infra |
| production, unattended work, cron, monitoring | Observability |
| AI, LLM, agent, prompt, model, RAG, tool use | AI/LLM |
| mobile, iOS, Android, offline, PWA | Mobile |
| onboarding, workflow, CLI, user-facing errors | Product/UX |
| scale, latency, traffic, token spend, cloud spend | Cost/Performance |
| personal data, health, finance, minors, licensing | Compliance |
| long-lived, team-owned, maintained systems | Team/Maintenance |

If a domain reveals another dependency during reasoning, Socratic expands the set mid-task.

That is important. A real task is often wider than the first user message suggests.

## Self-interrogation contract

Before implementation, Socratic pushes the model to arrive at a compact internal contract like this:

```text
Domains considered: what mattered and why
Self-answered highlights: the key decisions already resolved
Assumed (flag if wrong): important defaults taken
Open questions for you: only authority decisions
Top risks: the main engineering risks
Plan: what will be built
```

That is the real output of the questioning loop.

Not the raw 697 questions.

## Design principles

- Self-answer by default.
- Read before assuming.
- Assume before asking.
- Ask the user only when authority is required.
- Load the smallest sufficient context.
- Escalate from Core to Full only when the task justifies it.
- Keep testing and verification attached to the build.
- Treat questions as execution scaffolding, not conversation filler.

## Installation

### Claude Code

```bash
mkdir -p ~/.claude/skills
cp -r socratic ~/.claude/skills/
```

### Codex

Install into `$CODEX_HOME/skills`.

If `CODEX_HOME` is unset, use `~/.codex/skills`.

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -r socratic "${CODEX_HOME:-$HOME/.codex}/skills/"
```

Invoke explicitly with `$socratic`, or let it trigger implicitly when the task matches.

## Portable prompts

Socratic also includes portable prompts:

- [`PROMPT.md`](PROMPT.md): stronger, fuller version
- [`PROMPT_LITE.md`](PROMPT_LITE.md): lighter version for smaller tasks or tighter token budgets

Use them when you want the behavior without installing the full skill.

## When to use it

Use Socratic when the task is:

- vague
- underspecified
- multi-step
- production-sensitive
- cross-domain
- agentic
- likely to hide security, testing, or architecture risks

It is especially useful for:

- system design
- backend services
- agentic tools
- APIs and connectors
- AI product builds
- security-sensitive work
- production planning
- codebase-aware implementation

## When not to use it

Do not force it onto trivial work.

For example, it is probably unnecessary for:

- tiny copy fixes
- isolated one-line edits
- obvious local refactors
- purely mechanical changes

The point is to improve task completion quality, not to add ceremony.

## Extending it

To add a new domain:

1. Add a new full question file under `questions/`.
2. Add a matching compact file under `questions/core/`.
3. Follow the same structure:
   - Priority 1
   - themed sections
   - Verification
4. Add signal-routing logic in `SKILL.md` and `PROMPT.md`.

If you add a domain without a matching core file, default behavior becomes less efficient.

## Roadmap direction

A natural next step is knowledge-backed question packs.

That means using books, standards, postmortems, and domain-specific sources to curate better questions and better trade-off detection.

Examples:

- DDIA-backed system design questions
- threat-modeling-backed security questions
- SRE/postmortem-backed reliability questions
- LLM-eval-backed agent questions

That keeps Socratic as the loop engine while improving the depth of its internal questions.

## License

MIT. See [LICENSE](LICENSE).

## Contributing

PRs are useful for:

- new domains
- better signal routing
- stronger verification blocks
- high-signal core reductions
- questions that came from real incidents
- domain packs backed by strong engineering sources
