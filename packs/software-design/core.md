# A Philosophy of Software Design: Core pack

Use this pack for complexity, module boundaries, interface design, and accidental generality.

## Is this design reducing complexity or relocating it?

**Default answer pattern:** Prefer the design that removes required knowledge from callers and makes the common path obvious.

**Tradeoffs:** A deeper module may require more internal implementation, but it lowers system-wide cognitive load.

**Anti-patterns:** Moving validation, ordering rules, or edge-case coordination into every caller.

**Escalate when:** The abstraction hides a domain rule that callers must intentionally control.

**Verify:** Show that a new caller can use the module without knowing its internal coordination rules.

## Is this module deep enough to justify its interface?

**Default answer pattern:** Give modules a narrow interface with meaningful internal work behind it.

**Tradeoffs:** A small API can limit unusual use cases; expose extension points only when evidence requires them.

**Anti-patterns:** Thin wrappers, pass-through layers, or APIs that mirror storage and implementation details.

**Escalate when:** Different consumers need incompatible behaviours that cannot be expressed safely through one contract.

**Verify:** Count how much behaviour and policy the interface hides rather than exports.

## Is the interface forcing users to understand implementation details?

**Default answer pattern:** Model the caller's goal, not the module's internal steps.

**Tradeoffs:** Goal-oriented APIs may need clearer errors and documentation for exceptional paths.

**Anti-patterns:** Required call ordering, boolean option piles, or exposing internal IDs and lifecycle states without need.

**Escalate when:** The caller legitimately owns sequencing or policy decisions.

**Verify:** Write a usage example that reads like the caller's intent rather than a procedure manual.

## Are we adding generality before we have evidence we need it?

**Default answer pattern:** Implement the concrete case cleanly, then generalise from demonstrated variation.

**Tradeoffs:** Later refactoring is sometimes necessary; premature flexibility permanently raises complexity.

**Anti-patterns:** Generic frameworks for one use case, configuration for imagined futures, and speculative plug-in systems.

**Escalate when:** A confirmed near-term second use case has incompatible requirements.

**Verify:** Name the concrete current users and the observed variation each abstraction serves.

## Are names and comments carrying essential design meaning?

**Default answer pattern:** Use precise names and comments to explain non-obvious intent, invariants, and tradeoffs.

**Tradeoffs:** Documentation must be maintained, but ambiguous design costs every future reader.

**Anti-patterns:** Comments that restate code, vague names such as `Manager`, or missing rationale for surprising constraints.

**Escalate when:** A confusing API may need simplification rather than more explanation.

**Verify:** Ask whether a maintainer can explain why the design exists, not merely what each line does.
