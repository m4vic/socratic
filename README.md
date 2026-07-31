<p align="center">
  <img src="assets/pix.png" alt="Socratic Logo" width="180">
</p>

<h1 align="center">Socratic</h1>

<p align="center">
  Meta — questioning about questioning. Question yourself till you're left with only answers.
</p>

**Socratic is a self-interrogation skill for agentic AI systems.**

It helps an LLM slow down, inspect the task, ask itself the right engineering questions, and only ask the user for the few decisions that actually require human authority.

Not a form. Not a checklist. A lightweight reasoning loop.

## How it works

Instead of jumping straight into code, Socratic makes the model:

1. classify the task
2. detect the relevant domains
3. load the smallest useful question set
4. self-interrogate silently
5. surface assumptions, risks, and only necessary open questions
6. build
7. verify

The model interviews itself, not you.

## Core and Full

Socratic has two depths:

- `core`: the default, high-signal subset for routine, prototype, and medium-scope work
- `full`: the complete domain files for production, high-risk, audit-style, or security-sensitive work

The full bank has 697 questions. `core` keeps the default path practical by loading only the most important subset first.

## Domains

The full bank currently covers:

- Requirements & scope
- Frontend & UI
- Backend & services
- Data & storage
- API design
- Security
- Infrastructure & DevOps
- Testing & quality
- Observability & ops
- AI / LLM / agents
- Mobile & offline
- Product & UX
- Cost & performance
- Compliance & legal
- Team & maintenance

Each file starts with high-priority questions and ends with verification.

## Why it exists

Most agentic systems fail because they start executing too early.

They miss trade-offs, edge cases, testing gaps, security implications, and operational constraints. Socratic adds a structured internal loop before execution so the model reasons more like an engineer and less like autocomplete.

## Installation

### Claude Code

```bash
mkdir -p ~/.claude/skills
cp -r socratic ~/.claude/skills/
```

### Codex

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -r socratic "${CODEX_HOME:-$HOME/.codex}/skills/"
```

Invoke explicitly with `$socratic`, or let it trigger implicitly.

## Portable prompts

- [`PROMPT.md`](PROMPT.md)
- [`PROMPT_LITE.md`](PROMPT_LITE.md)

Use them when you want the behavior without installing the full skill.

## Extending

To add a new domain:

1. add a full file under `questions/`
2. add a matching compact file under `questions/core/`
3. update the routing logic in `SKILL.md` and `PROMPT.md`

## License

MIT. See [LICENSE](LICENSE).
