---
name: socratic
description: This skill should be used when the user asks to "build", "design", "scaffold", "architect", or "plan" any system, feature, service, app, agent, pipeline, connector, or tool — especially when the request is short, vague, or underspecified. It should also be used when the user asks to "review this architecture", "what am I missing", "ask me the right questions", "poke holes in this", or requests a design review before implementation. The skill interrogates the design silently across the relevant engineering domains (requirements, frontend, backend, data, API, security, infra, testing, observability, AI/LLM, mobile, product, cost, compliance, maintenance), resolves what the codebase and engineering defaults can answer, and surfaces only the decisions that require the user's authority.
---


# Socratic

Question yourself till you're left with only answers.

A curated bank of 697 questions a senior engineer asks before and during a build, split by domain. **The default mode is self-interrogation, not interviewing the user.** Read the codebase, apply engineering defaults, reason through the relevant questions, and ask the user only for decisions nobody but they can make.

## Two modes

**Mode A — Self-interrogation (default).** Ask and answer the relevant questions internally, then show the resulting contract once.

**Mode B — Interactive interview (opt-in).** Ask the user one yes/no question at a time. Enter this mode only when the user asks to be interviewed, walked through the decisions, or questioned one at a time.

If unsure, use Mode A. Silence is not a request to be interviewed.

## Mode A — Self-interrogation

### 1. Build the working domain set dynamically

For every build, include Requirements and Testing. Then inspect both the request and the existing project for domain signals:

| Signal in the request or project | Domain file |
|---|---|
| UI, page, component, dashboard, form, frontend | `01-frontend.md` |
| service, endpoint, job, queue, backend, business logic | `02-backend.md` |
| database, schema, storage, persistence, migration, cache | `03-data.md` |
| API, SDK, webhook, connector, integration, OAuth | `04-api.md` |
| authentication, accounts, payments, secrets, external input, public exposure | `05-security.md` |
| deployment, CI/CD, containers, cloud, scaling | `06-infra.md` |
| production, unattended work, cron, monitoring | `08-observability.md` |
| AI, LLM, agent, prompt, model, RAG, tool use | `09-ai-llm.md` |
| mobile, iOS, Android, offline, PWA | `10-mobile.md` |
| human-facing workflow, onboarding, errors, CLI | `11-product-ux.md` |
| scale, latency, traffic, token or cloud spend | `12-cost-performance.md` |
| personal or regulated data, health, finance, minors, licensing | `13-compliance.md` |
| maintained, long-lived, or team-owned work | `14-team-maintenance.md` |

Do not stop at the first match. A connector, for example, normally pulls in API, Security, and Testing. If answering one domain reveals another dependency, add that domain and continue until a complete pass reveals no new domains.

### 2. Load the smallest sufficient base question depth

Keep the user-facing behavior unchanged while controlling context use:

- **Core (default):** For routine, prototype, internal, or moderately scoped work, read the matching files under `questions/core/`. Always include `questions/core/00-requirements.md` and `questions/core/07-testing.md`.
- **Full:** For production systems, external users, public APIs, authentication, money, PII, regulated data, autonomous tools, costly or irreversible actions, or an explicit deep/full/audit request, read the matching complete files directly under `questions/`. Always include the complete Requirements and Testing files.
- **Escalation:** If a core answer exposes a serious security, reliability, compliance, scale, or operational risk, replace that core domain with its complete file before building.

Treat `$socratic lite` or `$socratic quick` as an explicit Core request. Treat `$socratic deep`, `$socratic full`, and `$socratic audit` as explicit Full requests. Never load all fifteen complete files unless the system genuinely spans all fifteen domains.

Questions consume context even when asked silently. Optimize for material risks resolved, not the number of questions processed.

### 3. Add specialized knowledge packs only when they sharpen the task

Base domain files stay primary. Packs are optional overlays, not replacements.

- Use a pack when the task clearly maps to a specialized body of tradeoffs, failure modes, or heuristics that the generic domain files do not capture well enough. First consult `packs/registry.md` after selecting the base domains.
- Load `packs/<pack>/core.md` after the base domain files. A pack may also provide `full.md`; load it only when the core pack proves insufficient for the decision at hand. Most packs ship core only.
- Prefer zero to two packs per task. Too many packs recreates the same token problem the Core/Full split was added to solve.
- Treat pack content as compact decision support: question, default answer pattern, tradeoffs, anti-patterns, escalation triggers, and verification checks.
- Select a pack by the capability it adds, not by recognising a book title. Its source material is provenance, not the routing rule.
- For pack structure or naming guidance, read `references/knowledge-pack-architecture.md`.

Examples:

- Use `packs/software-design/core.md` when reviewing complexity, module boundaries, interface design, or accidental generality.
- Use `packs/domain-modeling/core.md` when carving a system into boundaries, naming concepts, or deciding what must stay consistent together.
- Use `packs/data-systems/core.md` when reviewing durable state, consistency, queues, retries, migrations, or failure recovery.
- Use `packs/operations/core.md` when the work must survive production — timeouts, retries, load shedding, rollback, or alerting.
- Use `packs/threat-modeling/core.md` when mapping trust boundaries, attacker paths, abuse cases, mitigations, or security verification.
- Use `packs/ai-engineering/core.md` when building an LLM product, RAG system, model evaluation, or tool-enabled workflow.
- Use `packs/agent-design/core.md` when building an agent or subagent, splitting work across agents, setting tool permissions, or deciding how agent output gets verified.
- Use `packs/legacy-change/core.md` when modifying code that already works, has no tests, or is being replaced incrementally.
- Use `packs/testing-design/core.md` when deciding what to test, what to mock, or why a suite is brittle or untrusted.
- Use `packs/product-discovery/core.md` when the value of the thing itself is unproven — before the engineering packs, not alongside them.

New packs follow the same overlay pattern. Consult `packs/registry.md` for pairings and for choosing between adjacent packs.

### 4. Self-answer the selected material questions

Resolve each selected question in this order:

1. **Read first.** Use the codebase, configuration, documentation, prior conversation, and repository conventions.
2. **Apply the engineering default.** When evidence does not decide an engineering choice, take a defensible default and record consequential assumptions.
3. **Escalate only authority decisions.** Ask the user only about business priorities, budget, vendor choice, target market, legal risk tolerance, or irreversible decisions that materially change the result.

Do this silently. The user should see the resulting decisions, not the raw question bank.

### 5. Run a sufficiency check and stop deliberately

Socratic is not an instruction to exhaust a questionnaire. Its purpose is to reduce material uncertainty until the agent has a solid, evidence-backed basis to act.

After each domain or decision cluster, stop expanding the review when all of the following are true:

1. The requested outcome, scope, and consequential assumptions are clear.
2. Every material risk has a mitigation, a verification step, an explicit acceptance by the appropriate authority, or a clearly stated escalation.
3. No unresolved contradiction changes the implementation plan.
4. The next plausible question would not materially change the design, risk, cost, authority decision, or verification plan.
5. The plan has a proportionate way to falsify its riskiest assumptions.

If any condition fails, add the smallest relevant domain, deeper question depth, or pack. Do not continue merely to use more questions, and do not stop merely because a token budget is low. Stop because the remaining uncertainty is immaterial to the current task.

### 6. Emit the output contract once

Before implementation, emit:

```text
Domains considered: <each selected domain and why>
Self-answered highlights: <5-10 decisions that shaped the design>
Assumed (flag if wrong): <consequential defaults>
Open questions for you: <ideally 0-3 authority decisions>
Top risks: <material risks from the selected domains>
Plan: <what will be built>
```

Batch any genuinely blocking open questions. Do not turn the interrogation into the deliverable.

### 7. Build and verify

Build after blocking decisions are resolved. Apply the Verification guidance from every loaded domain and pack, including anything added during the review. Report what passed, failed, or could not be verified without external access.

## Interactive mode

When the user explicitly requests an interview:

1. Build the same dynamic domain set and choose Core or Full using the same rules.
2. Ask one material decision at a time with a recommended default.
3. On a correction, absorb it and continue. If the user says to stop or proceed, self-answer the remainder and build.
4. Keep the interaction proportional: 0-2 questions for a one-off, 3-6 for a prototype, 8-15 for production, and 15-25 for money, PII, health, or regulated systems.
5. Emit the same output contract before building.

## Preset domain combinations

Use these as a sanity check, not a fixed router:

| Building | Expected domains |
|---|---|
| CRUD web app | 00, 01, 02, 03, 05, 07 |
| Public API or SDK | 00, 02, 04, 05, 08, 12, 07 |
| Connectors or integrations | 00, 04, 05, 07, plus each connector's dependencies |
| AI agent or chatbot | 00, 09, 05, 12, 08, 11, 07 |
| RAG pipeline | 00, 09, 03, 12, 13, 07 |
| Data pipeline or ETL | 00, 03, 06, 08, 13, 07 |
| Mobile app | 00, 10, 01, 05, 11, 07 |
| Infrastructure change | 00, 06, 08, 12, 14, 07 |
| Payments feature | 00, 02, 03, 05, 13, 07 |
| One-off script | 00, 07 |

## Guardrails

- Do not ask what project inspection can answer.
- Do not expose the full question bank to the user by default.
- Do not escalate engineering decisions merely to avoid responsibility.
- Do not keep interviewing after the user asks to proceed.
- Do not claim verification without evidence.
- Do not present legal conclusions; identify matters requiring counsel.
- Do not depend on vendor-specific planning modes, memory systems, tool names, or subagent APIs.
