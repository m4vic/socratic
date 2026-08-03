# Agent design: Core pack

Use this pack when building an agent, a subagent, a skill that dispatches work, or any multi-agent workflow.

Unlike the other packs, this one is derived primarily from what shipped agent systems actually do — 34 agents published across first-party Claude Code plugins — rather than from a book. Structure observed in production beats structure argued from first principles.

## Should this be one agent or several?

**Default answer pattern:** One agent until a second *responsibility* appears, not a second step. Split when two parts of the work need different tool permissions, different models, or genuine independence of judgment.

**Tradeoffs:** Each agent boundary costs a context handoff — the second agent knows only what it is told. That cost buys isolation: a restricted tool set, an independent opinion, or a bounded blast radius.

**Anti-patterns:** Splitting by pipeline stage when every stage needs the same context. Spawning an agent for work the caller could do in three tool calls. A "coordinator" that only forwards messages.

**Escalate when:** The split is being made to work around context limits rather than to separate responsibility — that is a summarisation problem, not an architecture one.

**Verify:** State what each agent is allowed to do that the others are not. If the answer is "nothing", it should not be a separate agent.

## Should the agent that did the work also check it?

**Default answer pattern:** No. Separate the doer from the checker, always. Shipped systems do this without exception: one agent generates a patch and a different one verifies it; one finds candidate vulnerabilities and another votes on each.

**Tradeoffs:** A second pass costs latency and tokens. It buys the only opinion in the system that is not invested in the work being correct.

**Anti-patterns:** Asking a generator to "double-check its output". Letting the verifier see the generator's reasoning before forming its own view. A verifier that can edit — once it can fix, it stops reporting.

**Escalate when:** The verifier keeps agreeing. Either it is not independent, or it lacks the evidence to disagree.

**Verify:** Give the checker a known-bad output and confirm it rejects it. A verifier never tested against a failure has never been tested.

## What tools should this agent have?

**Default answer pattern:** Enumerate the minimum explicitly, never inherit the default set. Read-only agents get read and search tools and nothing else. Only agents whose job is to change things get write access.

**Tradeoffs:** Tight permissions mean an agent occasionally cannot finish and must hand back. That is the intended failure — it is visible, and it is recoverable.

**Anti-patterns:** Granting write access "in case it needs it". Giving an explorer a shell. Leaving tools unspecified so the agent silently inherits everything the parent can do.

**Escalate when:** An agent needs credentials, network access, or the ability to delete. Those cross a trust boundary and belong to the user's decision, not the design's.

**Verify:** Read the declared tool list and name the worst thing the agent could do with it. If that outcome is unacceptable, the list is wrong.

## Which model should this agent run?

**Default answer pattern:** Tier by how much judgment the task needs. Mechanical scanning, extraction, and structured search run on a small fast model. Review, adversarial critique, and orchestration run on the strongest available. Inherit from the caller when the right choice depends on what the caller is doing.

**Tradeoffs:** A cheaper model on a judgment task fails quietly — it produces fluent, plausible, wrong output that reads exactly like success.

**Anti-patterns:** One model for every agent because it is simpler. Choosing the cheap model for a verifier, which is the one role where being wrong is worst.

**Escalate when:** Cost pressure is pushing a judgment role onto a smaller model. That is a budget decision with a quality consequence, and it belongs to the user.

**Verify:** Run the agent's hardest realistic case on the cheaper model and compare. Assume nothing from the model's general reputation.

## Is this agent user-facing or dispatched?

**Default answer pattern:** Most agents are dispatched by a workflow, not invoked directly, and their description should say so plainly. Exactly one entry point should be user-facing.

**Tradeoffs:** A restricted, dispatched agent gives sharper results because it can assume the context its dispatcher guarantees. It becomes useless when called cold.

**Anti-patterns:** Every agent written as if a user might call it, so each re-establishes context the dispatcher already had. Two agents that both believe they are the entry point.

**Escalate when:** Users are invoking an internal agent directly. Either the entry point is missing something they need, or the internal agent should be promoted and hardened.

**Verify:** Invoke a dispatched agent cold. It should either state what it is missing or decline — never guess.

## How will anyone know the agent worked?

**Default answer pattern:** Define the agent's output shape and its failure statement before writing its instructions. An agent that cannot say "I could not determine this" will fabricate instead.

**Tradeoffs:** Structured output constrains what the agent can express, and some genuinely useful nuance gets flattened.

**Anti-patterns:** Free-form prose that the caller must parse. Confidence language with no evidence attached. Success measured by whether the agent finished rather than whether it was right.

**Escalate when:** The agent's output feeds an automated action with no human in the loop. Wrong output stops being an inconvenience and becomes an incident.

**Verify:** Run it on a case where the correct answer is "not enough information". Silence or a confident guess both mean it is not ready.
