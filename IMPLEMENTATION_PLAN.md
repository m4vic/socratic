# Socratic v1.0 implementation plan

## Purpose

Socratic helps an agent ask itself the smallest set of relevant questions needed to reach a solid answer and act. It is a self-interrogation and stopping system, not a checklist to exhaust and not an open-ended reasoning loop.

The desired outcome is an agent that can inspect a task from the right directions, resolve material uncertainty with evidence or defensible defaults, surface only authority decisions, and stop when further questioning would not improve the plan.

## Product contract

For a non-trivial task, Socratic must:

1. Select only domains relevant to the task; Requirements and Testing remain the baseline for builds.
2. Choose Core, Full, and zero to two specialist packs in proportion to actual stakes.
3. Turn selected questions into decisions, assumptions, risks, verification steps, or authority escalations.
4. Reassess whether another question can materially change the design, risk, cost, authority decision, or verification plan.
5. Stop when the remaining uncertainty is immaterial and build or deliver the answer.
6. Report a compact contract rather than exposing a raw questionnaire.

## Decision loop

```text
Task + project evidence
        |
        v
Select relevant base domains and depth
        |
        v
Add at most two capability packs when they sharpen a decision
        |
        v
Self-answer with evidence, defaults, or authority escalation
        |
        v
Sufficiency check
  |-- material uncertainty remains --> add the smallest relevant review surface
  '-- sufficient to act -----------> emit contract, build, verify, stop
```

## Definition of sufficient

The agent may stop expanding its analysis only when all of these are true:

- The intended outcome, scope, and meaningful constraints are understood.
- Consequential assumptions are visible and defensible.
- Each material risk has a mitigation, a verification step, authorised acceptance, or a clear escalation.
- No unresolved contradiction changes the chosen approach.
- The highest-risk assumption has a proportionate falsification or verification step.
- The next plausible question would not materially change the implementation or delivery decision.

This is an evidence-based stopping condition. It is not a fixed count of questions, a confidence score, or a token limit.

## Information architecture

### Base question domains

`questions/` is the routing and coverage layer. It ensures the agent considers the broad engineering surface: requirements, backend, data, API, security, infrastructure, testing, operations, AI, product, cost, compliance, and maintenance.

`questions/core/` contains the compact default path. The top-level question files are the deeper path for production, high-impact, or explicit audit work.

### Capability packs

`packs/` is the specialist reasoning layer. Packs are selected by the capability they add, not by a book acronym. `packs/registry.md` maps task signals to packs and their usual base-domain companions.

Current packs:

- `software-design` — complexity, interfaces, abstraction depth, and accidental generality.
- `data-systems` — data ownership, consistency, retries, event handling, migrations, and recovery.
- `threat-modeling` — assets, trust boundaries, attacker paths, mitigations, and detection.
- `ai-engineering` — foundation-model fit, evaluations, retrieval, agents, tools, budgets, and traceability.

Each pack keeps source books in `sources.md` as provenance. Packs are compact decision cards, not copied chapters or a substitute for the base domains.

## v1.0 implementation scope

### Completed

- Core versus Full question depth.
- Dynamic base-domain selection.
- Capability-based pack registry and templates.
- Four Core packs with source provenance.
- Portable Standard and Lite prompts.
- Explicit sufficiency and stopping instructions in the skill, prompts, and README.

### Release validation

Before publishing v1.0:

1. Validate frontmatter and skill structure with `quick_validate.py`.
2. Run `git diff --check` and verify all local Markdown links resolve.
3. Dry-run routing against representative tasks:
   - a one-line low-risk fix: Core, no pack;
   - a public API with persistence: Full API, Security, Data, Testing plus `data-systems`;
   - an AI agent with external tools: Full AI/LLM, Security, Cost, Observability, Testing plus `ai-engineering` and `threat-modeling`;
   - a startup interview plan: Requirements/Product plus a future `startup-discovery` pack, not engineering packs.
4. Confirm each scenario reaches a clear stop condition rather than loading unrelated domains or all packs.
5. Review the final diff and README from a clean clone before release.

## Explicit non-goals for v1.0

- Do not automate model confidence or claim that an LLM can prove its own answer correct.
- Do not load every question, pack, or book-derived principle by default.
- Do not create separate agents or an orchestration framework yet.
- Do not replace security review, legal advice, domain experts, or empirical testing.
- Do not copy source books into the repository.

## Post-v1.0 roadmap

1. Add `startup-discovery` from customer-discovery sources.
2. Add `product-positioning` for ICP, alternatives, differentiated value, and category design.
3. Expand only proven packs with `full.md` decision clusters, beginning with `data-systems`.
4. Add scenario-based regression fixtures so future edits cannot break routing or stopping behaviour.
5. Consider specialised agents only after the base skill shows a clear limitation that a pack cannot solve.

## Release procedure

1. Create a release branch, commit the v1.0 scope, and open a pull request.
2. Merge only after the validation checklist is green.
3. Create an annotated `v1.0.0` tag on the merged commit.
4. Push the tag and publish a GitHub release summarising Core/Full loading, capability packs, portable prompts, and the evidence-based stopping rule.
