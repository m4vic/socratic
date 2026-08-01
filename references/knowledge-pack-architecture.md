# Knowledge-pack architecture

## Goal

Keep `questions/` as Socratic's general-purpose routing layer. Use `packs/` for compact, source-backed overlays that add specialist tradeoffs without loading a book or a large question bank for every task.

Good candidates include software design philosophy, data-intensive architecture, threat modelling, LLM and agent evaluation, reliability or postmortem patterns. Product and business packs are also valid when they support concrete decisions, such as startup validation, pricing, positioning, go-to-market, or agent workflow design.

## Loading order

1. Choose the base domains from `questions/`.
2. Choose Core or Full depth.
3. Read `packs/registry.md` and add zero to two relevant packs.
4. Self-answer using the base domains plus pack overlays.
5. Build and verify.

## Layout

```text
questions/
  ...
  core/...
packs/
  _template/
    core.md
    full.md
  software-design/
    core.md
  data-systems/
    core.md
```

## Pack shape

Each decision card should be short and actionable:

1. **Question**
2. **Default answer pattern**
3. **Tradeoffs**
4. **Anti-patterns**
5. **Escalate when**
6. **Verify**

Expand Full with more edge cases and verification depth, not with a book summary.

For a large source, design the pack in two intentional layers:

- **Core:** roughly 5-10 high-leverage decision cards that apply often and prevent expensive mistakes.
- **Full:** roughly 20-40 cards grouped into a few decision clusters. Load it only for an explicit deep review or when Core exposes a material risk.

For example, a future `data-systems/full.md` can cover data models and encoding, storage and retrieval, replication and partitioning, transactions and consistency, batch and stream processing, and derived data. It should remain a decision tool, not a chapter-by-chapter summary.

## Boundary

Base domains decide the broad review surface: API, security, data, testing, and so on. Packs add specialist reasoning inside that surface. A pack never replaces a base domain and should not silently pull in unrelated concerns.

Use stable, lowercase capability names such as `software-design`, `data-systems`, `threat-modeling`, `ai-engineering`, `startup-strategy`, and `marketing-strategy`. Do not use a book title or acronym as the routing name; record the books in the pack's provenance instead. Keep every pack at a predictable path: `packs/<name>/core.md` and, when justified, `packs/<name>/full.md`.

## Authoring guidance

Write decision support, not notes about a book. Make the desired default, tradeoff boundary, escalation condition, and proof of correctness explicit. This gives an agent a useful next move while keeping token use low.

For future automation, the routing can simply map task signals to base domains, packs, and depth. The consistent paths and headings are intentional.
