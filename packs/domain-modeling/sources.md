# Domain modeling: sources

**Primary sources:**

- Eric Evans, *Domain-Driven Design: Tackling Complexity in the Heart of
  Software* — bounded contexts, ubiquitous language, aggregates, entities
  and value objects, and the core/supporting/generic subdomain distinction.
- Vaughn Vernon, *Implementing Domain-Driven Design* — aggregate sizing rules,
  context mapping between teams, and when the patterns are not worth applying.

**Supporting material:**

- Sam Newman, *Building Microservices* — service boundaries drawn along
  bounded contexts rather than technical layers.
- John Ousterhout, *A Philosophy of Software Design* — already the source for
  the `software-design` pack. That pack covers module depth and interface
  cost; this one covers where the boundaries should fall in the first place.

The two packs answer adjacent questions and are usefully loaded together when
restructuring an existing system.

A future `full.md` could add decision clusters for context mapping patterns
(shared kernel, customer/supplier, anticorruption layer), domain events and
their contracts, repository and factory design, and strategies for extracting
a bounded context from a monolith. Add a card only when it changes a real
design choice or verification step.
