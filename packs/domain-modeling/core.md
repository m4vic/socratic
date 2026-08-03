# Domain modeling: Core pack

Use this pack when carving a system into boundaries, naming concepts, deciding what belongs together, or untangling a model where one word means three different things.

## Does this domain deserve deep modeling at all?

**Default answer pattern:** Model deeply only where the business actually competes. Sort each area into core (the differentiator), supporting (needed, not distinctive), and generic (buy or copy). Rich models belong in core; the rest gets the simplest thing that works.

**Tradeoffs:** Deep modeling in a supporting area buys nothing and costs permanently — every future change pays the abstraction tax.

**Anti-patterns:** Aggregates, value objects, and domain events applied uniformly across a CRUD admin panel. Building a bespoke model of a solved problem — billing, auth, notifications.

**Escalate when:** The team disagrees about which subdomain is core. That is a business strategy question, not a technical one, and modeling before it is settled encodes the wrong answer in code.

**Verify:** Name the area's classification and what a competitor would have to copy to match it. If nothing, it is not core.

## Is this one concept or two concepts sharing a name?

**Default answer pattern:** Assume a word means different things in different parts of the business until proven otherwise. "Customer" in billing and "customer" in support usually share a name and almost nothing else. Draw a boundary and let each side keep its own definition.

**Tradeoffs:** Two models mean translation at the boundary and some duplicated data. The alternative is one model carrying every field either side ever needed, which serves neither.

**Anti-patterns:** A shared canonical model that every team must extend. A single table with thirty nullable columns because each consumer needed three. Treating a naming collision as an integration requirement.

**Escalate when:** Merging or splitting the concept changes team ownership or a published contract.

**Verify:** Ask two teams to define the term without conferring. Different answers mean different concepts.

## What has to be consistent in one transaction?

**Default answer pattern:** Group into one unit only what must hold true together at every instant. Keep those units small — usually one root and the data that cannot be valid without it. Everything else becomes eventually consistent and references by identity.

**Tradeoffs:** Small units mean more coordination and a window where the system is observably inconsistent. Large units mean lock contention and transactions that grow until they fail under load.

**Anti-patterns:** A unit that grows to cover an entire object graph because a report needed a join. Enforcing an invariant across two units in the same transaction. Choosing boundaries from the UI's shape rather than the rules'.

**Escalate when:** A business rule genuinely spans two units. That is either a missing concept or an accepted eventual-consistency window, and the user decides which.

**Verify:** State the invariant each unit protects in one sentence. A unit with no invariant is a table, not a boundary.

## Does this thing have identity, or is it just a value?

**Default answer pattern:** If two instances with identical fields are interchangeable, it is a value — make it immutable and compare by content. If it stays the same thing while its fields change, it has identity.

**Tradeoffs:** Immutable values allocate more and can complicate persistence. They eliminate an entire category of aliasing bugs in return.

**Anti-patterns:** Giving every concept a database ID by reflex. Money, dates, ranges, and addresses modeled as mutable entities. Primitive strings and decimals standing in for concepts with real rules.

**Escalate when:** Something modeled as a value turns out to need an audit trail. It has identity after all.

**Verify:** Swap one instance for an equal one. If anything downstream notices, it has identity.

## Where does this behavior belong?

**Default answer pattern:** Put behavior on the concept that owns the data it decides on. Reach for a separate service only when an operation genuinely belongs to no single concept.

**Tradeoffs:** Rich models are harder to serialize and can tangle with the persistence layer. Anemic models are trivially serializable and push every rule into procedures that drift apart.

**Anti-patterns:** Objects that are only getters and setters, with all logic in a `Manager` or `Service`. The same validation implemented differently in three call sites. A domain service that is a bag of unrelated functions.

**Escalate when:** Behavior needs data from another boundary to decide. That is a signal the boundary is wrong or a concept is missing.

**Verify:** Find a business rule and count where it is enforced. More than one place means it is on the wrong object.

## Are the code and the conversation using the same words?

**Default answer pattern:** Use the domain's language in class and method names exactly as practitioners say it. When the code needs a word the business does not use, the model is missing a concept or inventing one.

**Tradeoffs:** Domain language can be verbose and occasionally collides with language keywords. It removes the translation step every reader otherwise performs.

**Anti-patterns:** `OrderData`, `ProcessManager`, `handleRequest2`. Technical names for domain concepts. A glossary nobody has read since it was written.

**Escalate when:** The business uses a word inconsistently. Fix the vocabulary before the code — the ambiguity will otherwise be encoded permanently.

**Verify:** Read a method name aloud to someone in the business. If it needs translating, rename it.
