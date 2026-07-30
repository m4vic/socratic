# Socratic

*Meta — questioning about questioning. Question yourself till you're left with only answers.*

**697 questions a senior engineer asks before writing code — packaged as a Claude skill, a portable prompt, and a plain checklist.**

LLMs and coding agents tend to start typing immediately. This makes them stop and interrogate the request first, the way an experienced engineer would: one yes/no question at a time, routed to the domains that actually apply, stopping the moment you say "enough, just build it."

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

Every file follows the same shape: **Priority 1** questions first (ask these or nothing), then thematic sections, then a **Verification** block to run *after* the build.

## Three ways to use it

### 1. As a Claude skill

Copy the folder into your skills directory:

```bash
# Claude Code / Cowork — project-level
mkdir -p .claude/skills
cp -r socratic .claude/skills/

# or user-level, available everywhere
cp -r socratic ~/.claude/skills/
```

Then just ask for something:

> "Build me a service that lets users upload CSVs and get a summary emailed to them."

The skill triggers, picks the relevant domains, and starts asking.

### 2. As a portable prompt

Paste [`PROMPT.md`](PROMPT.md) into the system prompt of any LLM — ChatGPT, Gemini, a local model, your own agent framework. It's self-contained and doesn't reference the question files.

For the full bank in one file, concatenate:

```bash
cat PROMPT.md questions/*.md > everything.md
```

### 3. As a human checklist

Open the domain file before a design review, an architecture doc, or a PR. The Verification sections work well as a pre-merge checklist.

## The interrogation loop

```
Classify request → pick 2-5 domains
   ↓
Ask ONE yes/no question (with a proposed default)
   ↓
Yes → next question        No → absorb correction, or STOP and build
   ↓
Budget reached → emit contract (confirmed / assumed / out of scope / risks / plan)
   ↓
Build
   ↓
Run Verification sections of the domains used
```

**Question budget by stakes:**

| Stakes | Max questions |
|---|---|
| Throwaway script | 0–2 |
| Prototype / internal tool | 3–6 |
| Production, real users | 8–15 |
| Money, PII, or health data | 15–25 (security bank mandatory) |

## Preset stacks

Skip routing — grab a combo:

| Building | Load |
|---|---|
| CRUD web app | 00, 01, 02, 03, 05, 07 |
| Public API / SDK | 00, 02, 04, 05, 08, 12 |
| Internal dashboard | 00, 01, 03, 11 |
| AI agent / chatbot | 00, 09, 05, 12, 08, 11 |
| RAG pipeline | 00, 09, 03, 12, 13 |
| Data pipeline / ETL | 00, 03, 06, 08, 13 |
| Mobile app | 00, 10, 01, 05, 11 |
| Infra change | 00, 06, 08, 12, 14 |
| Payments feature | 00, 02, 03, 05, 13, 07 |
| One-off script | 00 only |

## Design principles

- **One question per turn.** A numbered list of 40 questions is a form, not a conversation. Nobody fills it in.
- **Always propose a default.** "I'll rate-limit at 100/min per IP — OK?" gets an answer. "What rate limit?" gets a shrug.
- **Highest-rework-cost first.** Data model and auth questions come before styling questions.
- **Read before asking.** Anything inferable from the repo isn't a question.
- **"No" means stop.** Impatience is a signal, not an obstacle. So is a one-word answer twice in a row.
- **Never ask a question whose answer doesn't change the code.**

## Extending it

Add a domain by dropping `questions/15-yourdomain.md` in, following the existing shape (Priority 1 → sections → Verification), and adding a row to the routing table in `SKILL.md`. Keep questions concrete enough that a wrong answer changes the design.

## License

MIT — see [LICENSE](LICENSE). Use it, fork it, ship it.

## Contributing

PRs welcome, especially:

- Domains not covered (embedded, games, blockchain, hardware, accessibility-in-depth)
- Questions that came from a real incident — those are the good ones
- Corrections where a question encodes an outdated practice
