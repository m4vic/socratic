# Source provenance

Primary source: *Designing Data-Intensive Applications* by Martin Kleppmann.

This pack is a compact interpretation of distributed-data design patterns. It does not reproduce the book. Use the task's invariants and operational constraints to choose the appropriate tradeoffs.

## Intended coverage

`core.md` contains the most broadly reusable operational decisions: data ownership, consistency, retry safety, event ordering, change management, and partial-failure recovery.

A future `full.md` should add deeper decision clusters for data models and encoding, storage and retrieval, replication and partitioning, transactions, batch and stream processing, and derived data. Add a card only when it changes a real design choice or verification step.
