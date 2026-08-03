# Agent design: sources

**Primary source: shipped agent systems.** Structure observed across 34 agents
published in first-party Claude Code plugins — `claude-security`,
`code-modernization`, `pr-review-toolkit`, `feature-dev`, `plugin-dev`,
`skill-creator`, and `hookify`.

Seven archetypes recur: read-only explorer, adversarial critic, verifier,
generator, orchestrator, extractor, and grader. The decision cards in
`core.md` come from what those systems consistently do — separating the doer
from the checker, enumerating tools explicitly, tiering the model by required
judgment, and marking most agents as dispatched rather than user-facing.

**Supporting material:**

- Anthropic, *Building Effective Agents* and the Claude Code plugin and skill
  authoring documentation — progressive disclosure, trigger-phrase
  descriptions, and the boundary between skills, agents, and scripts.
- Zhou et al., *TRACE: Compiling User Corrections into Runtime Enforcement for
  Coding Agents*, arXiv 2606.13174 — the access-versus-compliance distinction:
  making a constraint visible to an agent does not make it obeyed.
- Chip Huyen, *AI Engineering* — evaluation and failure analysis, already the
  source for the `ai-engineering` pack. That pack covers the model layer;
  this one covers the orchestration above it.

A future `full.md` could add decision clusters for context handoff between
agents, retry and escalation policy, cost attribution across a multi-agent
run, and evaluating an agent system end to end rather than per agent. Add a
card only when it changes a real design choice or verification step.
