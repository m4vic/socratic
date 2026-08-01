# AI engineering: Core pack

Use this pack with the AI/LLM domain for LLM products, RAG systems, agents, and tool-enabled workflows.

## Is a foundation model the right component for this outcome?

**Default answer pattern:** Define the user outcome, acceptable error, cost, latency, and data constraints before choosing a model or agent architecture. Prefer deterministic software for deterministic rules.

**Tradeoffs:** Models handle ambiguity and unstructured input well; they are probabilistic, can drift, and often cost more than direct logic.

**Anti-patterns:** Adding an agent because it is fashionable, using a model for fixed classification rules, or treating a fluent response as proof of task success.

**Escalate when:** A wrong output can move money, alter permissions, give regulated advice, publish externally, or cause irreversible actions.

**Verify:** Compare a model-based approach with the simplest deterministic or human-assisted alternative against the same success metric.

## What behaviour will prove the system is good enough to ship?

**Default answer pattern:** Build a representative, versioned evaluation set before tuning prompts, retrieval, models, or agent workflows. Include normal, difficult, adversarial, and refusal cases.

**Tradeoffs:** High-quality evaluation data takes effort; optimisation without it produces demos that cannot be trusted in production.

**Anti-patterns:** Evaluating only a few hand-picked examples, changing prompts and test cases together, or using one aggregate score that hides harmful failures.

**Escalate when:** Evaluation requires expert judgement, sensitive data, safety review, or a metric that meaningfully affects product access.

**Verify:** Run the fixed evaluation set for every material model, prompt, retrieval, or tool-policy change and inspect failures by category.

## Where must the system be deterministic, structured, or able to refuse?

**Default answer pattern:** Put schemas, validators, policy checks, and deterministic business rules around the model. Give the model a safe abstention path when confidence or evidence is insufficient.

**Tradeoffs:** Guardrails may reduce conversational freedom; they prevent untrusted text from becoming an unchecked system action.

**Anti-patterns:** Parsing natural language with brittle string rules, accepting model output directly as an API command, or forcing the model to answer when it lacks evidence.

**Escalate when:** Output drives code, database writes, credentials, external communications, or legal, medical, financial, and security decisions.

**Verify:** Test invalid structured output, missing evidence, policy-denied requests, and conflicting instructions; confirm the system fails safely.

## Does retrieval improve the answer, and can we show where it came from?

**Default answer pattern:** Use retrieval only when fresh or private knowledge materially improves the task. Control document quality, access permissions, chunking, ranking, citations, and stale-content handling.

**Tradeoffs:** Retrieval can ground answers but adds latency, cost, access-control risk, and new failure modes such as irrelevant context.

**Anti-patterns:** Indexing every document without ownership or permission checks, assuming retrieved text is true, or presenting unsupported answers as grounded.

**Escalate when:** The corpus includes tenant data, regulated records, untrusted uploads, or instructions that can influence tool actions.

**Verify:** Evaluate answer quality with and without retrieval, test permission boundaries, and inspect whether citations actually support the generated claim.

## What can the model or agent do, and how is each action constrained?

**Default answer pattern:** Give tools narrow schemas, least privilege, explicit allowlists, scoped credentials, budgets, timeouts, and approval gates for high-impact actions.

**Tradeoffs:** More controls make agents less autonomous; unrestricted tool access makes prompt injection and model mistakes operational incidents.

**Anti-patterns:** A single all-powerful tool, secrets exposed in context, tool selection based only on user text, or unbounded loops and retries.

**Escalate when:** An action sends messages, changes data, spends money, accesses external systems, or affects more than one user or tenant.

**Verify:** Attempt unauthorized tool calls, harmful arguments, repeated actions, prompt-injected retrieved content, timeout, and budget-exhaustion paths.

## Is serving cost, latency, and reliability bounded under real usage?

**Default answer pattern:** Set explicit budgets for tokens, model calls, tool calls, elapsed time, concurrency, and fallback behaviour. Instrument each stage and degrade gracefully.

**Tradeoffs:** Lower-cost models and caching can reduce quality; unlimited reasoning and retries create surprise bills and queue collapse.

**Anti-patterns:** Measuring only average latency, no per-request budget, silently retrying expensive calls, or relying on a single provider without a failure plan.

**Escalate when:** The workload is public, unattended, customer-billed, high-volume, or exposed to adversarial traffic.

**Verify:** Load-test representative paths and failure paths; report p95 latency, token and tool cost, fallback rate, and budget-exhaustion behaviour.

## Can we reproduce and explain a production answer or action?

**Default answer pattern:** Version the model, system prompt, tool policy, retrieval configuration, evaluation set, and relevant code. Record privacy-safe traces sufficient to debug outcomes.

**Tradeoffs:** Traces improve accountability but can create retention and privacy obligations; log only what is necessary and protect it.

**Anti-patterns:** Shipping prompt changes without a version, logging raw sensitive conversations indefinitely, or being unable to associate a regression with a model/configuration change.

**Escalate when:** Logs contain personal data, customer secrets, regulated information, or content required for incident investigation.

**Verify:** Re-run a sampled production-like case from its versioned configuration and confirm trace access follows retention and permission rules.
