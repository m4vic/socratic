# Threat modeling: Core pack

Use this pack with the Security domain to turn a system design into explicit attacker paths, mitigations, and proof that the mitigations work.

## What are we protecting, and what harm matters most?

**Default answer pattern:** List the assets first: identities, credentials, money, private data, permissions, system availability, and integrity of important actions. Rank harm by impact, not by technical novelty.

**Tradeoffs:** A complete asset inventory takes time; an unranked list of vulnerabilities makes teams protect the wrong thing.

**Anti-patterns:** Starting with a favourite attack technique, treating every database field as equally sensitive, or defining security only as confidentiality.

**Escalate when:** The system handles regulated data, payments, safety-critical actions, privileged administration, or irreversible external effects.

**Verify:** Name an owner, classification, and unacceptable outcome for every high-value asset.

## Where are the trust boundaries and data flows?

**Default answer pattern:** Draw the system's actors, processes, data stores, external services, and every crossing where identity, privilege, or data trust changes.

**Tradeoffs:** A lightweight diagram can miss implementation details; it reveals risks that component lists and code review often hide.

**Anti-patterns:** Treating an internal network as inherently trusted, omitting webhooks and background jobs, or drawing only the happy path.

**Escalate when:** A boundary crosses tenants, organisations, devices, cloud accounts, third-party tools, or human approval steps.

**Verify:** Trace one sensitive request and one background action from origin to storage, including every boundary crossed.

## How could an attacker impersonate, tamper with, or replay this action?

**Default answer pattern:** For each sensitive flow, test identity binding, authorization, integrity checks, freshness, and replay resistance.

**Tradeoffs:** Stronger verification can add latency and operational complexity; it protects actions whose apparent source cannot be trusted.

**Anti-patterns:** Checking authentication but not object-level authorization, trusting client-supplied roles, accepting unsigned webhooks, or relying only on a timestamp without a replay policy.

**Escalate when:** The action changes permissions, moves money, exposes private data, invokes a tool, or affects another tenant.

**Verify:** Attempt a cross-user or cross-tenant request, modified payload, expired credential, and replayed valid request in tests.

## Can an untrusted input cross into a privileged interpreter or tool?

**Default answer pattern:** Treat external text, files, URLs, tool arguments, and retrieved content as untrusted. Constrain capabilities with allowlists, schemas, least privilege, and confirmation for high-impact actions.

**Tradeoffs:** Constraints reduce flexibility and may require explicit product decisions; unrestricted interpretation turns data into authority.

**Anti-patterns:** Passing user text directly to shells, queries, templates, browser automation, or agent tools; trusting retrieved content because it came from an internal source.

**Escalate when:** The system executes code, sends messages, changes records, accesses secrets, or acts as an AI agent.

**Verify:** Use adversarial inputs that attempt command injection, prompt injection, parameter smuggling, or unauthorized tool selection.

## If one identity, secret, or component is compromised, what is the blast radius?

**Default answer pattern:** Isolate tenants and privileges, scope credentials narrowly, rotate secrets, and make privileged actions attributable and reversible where possible.

**Tradeoffs:** Fine-grained permissions and key management add administration; shared superuser credentials turn small breaches into system-wide ones.

**Anti-patterns:** Long-lived shared tokens, broad database credentials, secrets in logs, or service accounts that can perform every action.

**Escalate when:** Credentials reach production data, deployment systems, financial accounts, model providers, or customer integrations.

**Verify:** Review effective permissions and demonstrate that a compromised component cannot access an unrelated tenant or high-impact operation.

## Which threats remain, who accepts them, and how will we detect them?

**Default answer pattern:** Record residual risks with an owner, rationale, expiry or review date, and observable signals for abuse or mitigation failure.

**Tradeoffs:** Some risk is unavoidable; undocumented risk becomes an accidental product decision and cannot be monitored.

**Anti-patterns:** Marking a finding "accepted" without authority, logging sensitive values to gain visibility, or treating a control as complete without detection.

**Escalate when:** A residual risk involves legal obligations, material customer harm, public exposure, or a strategic business tradeoff.

**Verify:** Confirm alerts, audit trails, and an incident response owner exist for high-impact residual risks.
