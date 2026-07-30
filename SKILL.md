---
name: socratic
description: Meta — questioning about questioning. Question yourself till you're left with only answers. Use whenever the user asks to build, design, scaffold, or architect any system, feature, service, app, agent, pipeline, or tool — especially when the request is short, vague, or underspecified. Routes to domain-specific question banks (frontend, backend, data, security, infra, AI/LLM, mobile, API, testing, product, cost, compliance) and asks one yes/no question at a time until the user says "no, just build it." Also use when reviewing an existing design for gaps, or when the user says "what am I missing," "review this architecture," or "ask me the right questions."
---

# Socratic

Question yourself till you're left with only answers.

A curated bank of ~700 questions a senior engineer asks before and during a build, split by domain. Not a checklist to fill out — a live interrogation loop the agent runs on itself, one question at a time, until nothing's left unexamined.

## Core loop

**One question per turn. Never batch.**

1. **Classify the request.** Read the task and pick the 2-5 relevant domains from the table below. Say which ones you picked in one line, then start.
2. **Ask question 1** from the highest-priority domain, phrased as a single yes/no check with your proposed default baked in:
   > "I'm assuming X. Is that OK?"
3. **Wait.**
   - **Yes / silence-equivalent** → next question.
   - **No** → the user is either correcting you or telling you to stop. If they gave a correction, absorb it and continue. If they said "no, just build it" / "that's enough" / "stop asking" → **stop the loop immediately and build** using answers so far plus documented defaults.
4. **Budget the loop.** Scale question count to stakes:

   | Stakes | Max questions |
   |---|---|
   | Throwaway script, one-off | 0-2 |
   | Prototype / internal tool | 3-6 |
   | Production, real users | 8-15 |
   | Handles money, PII, or health data | 15-25, security bank mandatory |

5. **Before building**, emit a short contract:
   ```
   Confirmed: ...
   Assumed (unasked): ...
   Out of scope: ...
   Top 3 risks: ...
   Plan: ...
   ```
6. **After building**, re-run the Verification section of each domain used. Report what passed, what you couldn't verify.

## Question phrasing rules

- Always propose a default. `"Should I rate-limit the endpoint at 100 req/min per IP — OK?"` beats `"What rate limit do you want?"`
- One decision per question. If you need two answers, that's two turns.
- Lead with the question that would cause the most rework if answered wrong. Security and data-model questions come before styling questions.
- Skip anything you can infer from the codebase, existing files, or earlier turns. Read first, ask second.
- Never ask a question whose answer doesn't change what you build. If both answers lead to the same code, don't ask.

## Domain routing

| Domain | Load when the task involves | File |
|---|---|---|
| Requirements & scope | Always. Start here. | `questions/00-requirements.md` |
| Frontend & UI | UI, web app, components, forms, dashboards | `questions/01-frontend.md` |
| Backend & services | APIs, services, business logic, jobs, queues | `questions/02-backend.md` |
| Data & storage | Databases, schemas, migrations, ETL, analytics | `questions/03-data.md` |
| API design | Public/internal APIs, SDKs, webhooks, integrations | `questions/04-api.md` |
| Security | Auth, user data, payments, external input, secrets | `questions/05-security.md` |
| Infra & DevOps | Deployment, CI/CD, containers, cloud, scaling | `questions/06-infra.md` |
| Testing & QA | Any code that will be maintained | `questions/07-testing.md` |
| Observability | Anything running in production | `questions/08-observability.md` |
| AI / LLM / agents | Prompts, RAG, agents, tool use, model calls, evals | `questions/09-ai-llm.md` |
| Mobile & offline | iOS, Android, React Native, offline-first, PWA | `questions/10-mobile.md` |
| Product & UX | User-facing anything; onboarding, errors, a11y | `questions/11-product-ux.md` |
| Cost & performance | Scale, latency budgets, cloud spend, API billing | `questions/12-cost-performance.md` |
| Compliance & legal | PII, health, finance, EU/CA users, licensing | `questions/13-compliance.md` |
| Team & maintenance | Anything outliving this session | `questions/14-team-maintenance.md` |

Read only the domain files you routed to. Loading all fifteen wastes context and produces bloated interrogations.

## Preset stacks

Common combos — use these instead of routing from scratch:

- **CRUD web app** → 00, 01, 02, 03, 05, 07
- **Public API / SDK** → 00, 02, 04, 05, 08, 12
- **Internal dashboard** → 00, 01, 03, 11, 05(light)
- **AI agent / chatbot** → 00, 09, 05, 12, 08, 11
- **RAG pipeline** → 00, 09, 03, 12, 13
- **Data pipeline / ETL** → 00, 03, 06, 08, 13
- **Mobile app** → 00, 10, 01, 05, 11
- **Infra / platform change** → 00, 06, 08, 12, 14
- **Payments or fintech feature** → 00, 02, 03, 05, 13, 07 (all mandatory)
- **One-off script** → 00 only, max 2 questions

## Anti-patterns

Do not:

- Ask all questions upfront as a numbered list. That's a form, not a conversation.
- Ask questions you could answer by reading the repo.
- Keep asking after the user signals impatience — "no," "just do it," "whatever you think," or a one-word answer twice in a row all mean **stop**.
- Ask about scale for a prototype, or about compliance for a personal script.
- Let the interrogation become the deliverable. The point is the system, not the questionnaire.

## Using this outside the loop

The banks work standalone as:

- **Design review checklist** — walk a finished design against the relevant domain files, list every unanswered question as a gap.
- **PR review prompt** — pull the Verification sections only.
- **Interview question source** — the banks are structured as things senior engineers actually ask.
