---
name: socratic
description: Meta — questioning about questioning. Question yourself till you're left with only answers. Use whenever the user asks to build, design, scaffold, or architect any system, feature, service, app, agent, pipeline, connector, or tool — especially when the request is short, vague, or underspecified. By default the agent interrogates ITSELF silently across the relevant engineering domains (frontend, backend, data, security, infra, AI/LLM, mobile, API, testing, product, cost, compliance) and reasons its way to answers, surfacing only the few decisions that truly need the user's input. Also use when reviewing an existing design for gaps, or when the user says "what am I missing," "review this architecture," or "ask me the right questions."
---

# Socratic

Question yourself till you're left with only answers.

A curated bank of ~700 questions a senior engineer asks before and during a build, split by domain. **The default mode is self-interrogation, not interviewing the user.** The agent runs the question bank on itself — reads the codebase, applies engineering defaults, reasons through each relevant question — and only turns to the user for the small number of decisions nobody but them can make.

## Two modes

**Mode A — Self-interrogation (default).** The agent asks itself, answers itself, shows its work once at the end. This is what runs unless the user asks otherwise.

**Mode B — Interactive interview (opt-in).** The agent asks the user one yes/no question at a time. Only enter this mode if the user says something like "ask me," "interview me," "walk me through it," or "let's go one at a time." See the Interactive Mode section at the bottom.

If unsure which the user wants, default to Mode A. Silence is not a request to be interviewed.

---

## Mode A — Self-interrogation

### 1. Build the working domain set (dynamic)

Always include:
- `questions/00-requirements.md`
- `questions/07-testing.md` — every build gets a testing pass, not just ones where testing was explicitly requested

Then scan the request **and** the existing codebase (if any) for signals, and add every domain that matches. Err toward including a domain over skipping it — self-answering extra questions is free; the user never sees the raw list.

| Signal in the request or code | Add domain(s) |
|---|---|
| UI, page, component, dashboard, form, "frontend" | `01-frontend.md` |
| service, endpoint, job, queue, "backend", business logic | `02-backend.md` |
| database, schema, store, persist, migration, cache | `03-data.md` |
| API, SDK, webhook, **connector**, integration, third-party service, OAuth | `04-api.md` |
| auth, login, user accounts, payments, secrets, external input, public-facing | `05-security.md` |
| deploy, CI/CD, container, cloud, scaling, "infra" | `06-infra.md` |
| production, "will run unattended", cron, monitoring | `08-observability.md` |
| AI, LLM, agent, prompt, model, RAG, tool-use, chatbot | `09-ai-llm.md` |
| mobile, iOS, Android, offline, PWA | `10-mobile.md` |
| anything user-facing at all | `11-product-ux.md` |
| scale, latency, "will this be expensive", high traffic | `12-cost-performance.md` |
| personal data, payments, health, users in EU/CA, minors | `13-compliance.md` |
| "will be maintained", team project, long-lived | `14-team-maintenance.md` |

Connectors and integrations are a good example of why this has to be dynamic: a "tool with connectors" isn't just an API question. It pulls in `04-api.md` (contract, auth to the third party, rate limits), `05-security.md` (secrets for each connector, what happens if one is compromised), and `07-testing.md` (mocking each connector, testing what happens when one is down) all at once. Don't stop at the first obviously-matching domain — check the whole table.

**This set is not fixed at the start.** If self-answering a question in one domain reveals a new characteristic — e.g. answering a backend question makes clear the system needs a persistent store — re-scan and pull in the newly relevant domain (here, `03-data.md`) before continuing. Keep doing this until a full pass produces no new domains.

### 2. Self-answer every question in the working set

For each question, resolve it yourself, in this order:

1. **Read first.** Check the codebase, existing config, prior conversation, or repo conventions. If the answer is already decided somewhere, use it — don't re-ask what you can look up.
2. **Apply the engineering default.** Most questions in the bank have an obvious, defensible default for the type of system being built (see each domain file's Priority 1 section for the highest-value ones). Take it. Record it as **Assumed**.
3. **Escalate only if neither applies AND the answer is a business/authority decision, not an engineering one** — budget, which vendor, target market, legal risk tolerance, an irreversible action, or something that materially changes what's being delivered. These become **Open questions for the user**. Everything else gets answered by you.

Do this silently. The user does not see 700 questions go by — they see the output contract below.

### 3. Emit the output contract once, before writing code

```
Domains considered: <list, one line each on why it was pulled in>
Self-answered highlights: <5-10 bullets — the decisions that most shaped the design, not all of them>
Assumed (flag if wrong): <the defaults you took>
Open questions for you: <ideally 0-3 — only things nobody but the user could decide>
Top risks: <one line each, from whichever domains flagged them>
Plan: <what you're about to build>
```

If "Open questions" is non-empty, ask them now — batched, not one at a time (batching is fine here because these are the rare genuinely-blocking ones, not the full interrogation).

### 4. Build

### 5. Verify

Run the **Verification** section of every domain in the final working set — not just the ones for domains the user explicitly asked about. `07-testing.md`'s verification list runs every time, since a tool was created. Report what passed and what couldn't be verified without the user's help (e.g., "can't load-test without your staging environment").

### Budget note

Self-interrogation is cheap — there's no reason to skip domains to save the user's time, since the user isn't in the loop for the silent pass. The place to economize is the **Open questions** list: keep that as close to zero as the task allows. A trivial script might have a working domain set of just `00` + `07` and zero open questions. A payments feature might pull in six domains and still land on 1-2 open questions (which processor, what's the fraud tolerance) — everything else gets a defensible default.

---

## Interactive Mode (opt-in — only when asked)

If the user asks to be interviewed instead of having the agent self-answer, switch to this loop:

1. Build the same dynamic domain set as Mode A.
2. Ask ONE question at a time, phrased with a proposed default: *"I'm assuming X. Is that OK?"*
3. **Yes** → next question. **No + correction** → absorb it, continue. **No / "just build it" / a one-word answer twice in a row** → stop immediately, fall back to Mode A for everything unanswered, and build.
4. Question budget by stakes: throwaway script 0-2, prototype 3-6, production 8-15, money/PII/health 15-25.
5. Emit the same output contract as Mode A before building.

## Anti-patterns

- Don't ask the user all 700 questions, batched or one at a time, by default. That's Mode B, and Mode B is opt-in.
- Don't skip the self-interrogation pass just because the task sounds simple — run it, it just resolves to a short domain set and an empty open-questions list.
- Don't surface an "Assumed" item as an "Open question" just to be safe. If there's a defensible engineering default, take it and move on.
- Don't let "Open questions for you" grow past a handful. If it's long, you're escalating engineering decisions that should have been self-answered.
- Don't skip Verification for domains you silently added — if you pulled in `05-security.md` because the task turned out to need auth, its verification checks run too.

## Preset domain combos (still useful as a sanity check on the dynamic scan)

| Building | Should end up with at least |
|---|---|
| CRUD web app | 00, 01, 02, 03, 05, 07 |
| Public API / SDK | 00, 02, 04, 05, 08, 12, 07 |
| Tool with connectors/integrations | 00, 04, 05, 07, + whatever each connector touches (data, AI, etc.) |
| AI agent / chatbot | 00, 09, 05, 12, 08, 11, 07 |
| RAG pipeline | 00, 09, 03, 12, 13, 07 |
| Data pipeline / ETL | 00, 03, 06, 08, 13, 07 |
| Mobile app | 00, 10, 01, 05, 11, 07 |
| Infra change | 00, 06, 08, 12, 14, 07 |
| Payments feature | 00, 02, 03, 05, 13, 07 (all mandatory) |
| One-off script | 00, 07 only |
