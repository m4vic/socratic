# 03 — Data & Storage

**Load when:** databases, schemas, migrations, caching, file storage, ETL, analytics, or anything that persists.

## Priority 1

1. What's the source of truth for each piece of data?
2. Relational, document, key-value, or something else — and why not the boring relational default?
3. What's the expected row/record count in year one, and in year three?
4. Read-heavy, write-heavy, or balanced?
5. Is any of this data sensitive or regulated? (If yes, also load `05-security.md` and `13-compliance.md`.)

## Schema design

6. What are the core entities and their relationships?
7. Which fields are genuinely required, and which are nullable?
8. Are there natural keys, or is everything surrogate IDs?
9. Should IDs be sequential integers or UUIDs — and does that leak information?
10. Are enums stored as strings, integers, or a lookup table? What happens when a value is added?
11. Is anything denormalized, and what keeps the copies in sync?
12. Are timestamps stored in UTC with timezone awareness?
13. Is money stored as integers/decimal rather than floats?
14. Are there JSON columns, and are you giving up queryability you'll want later?
15. Is soft-delete used, and does every query remember to filter deleted rows?

## Migrations

16. How are schema changes applied, and are they versioned in the repo?
17. Is every migration reversible, and has the down-migration been tested?
18. Can the migration run without downtime on a table of the expected size?
19. Is the code backward-compatible with the old schema during a rolling deploy?
20. Is there a data backfill, and can it run in batches without locking?
21. What's the plan if a migration fails halfway?

## Queries & performance

22. What are the top 5 queries by frequency, and are they indexed?
23. Is there an N+1 query hiding behind an ORM relation?
24. Are there queries without a LIMIT that could return unbounded rows?
25. Are indexes covering the actual WHERE + ORDER BY combinations used?
26. Is any query doing a full table scan on a table that will grow?
27. Are there long-running transactions holding locks?
28. Is connection pooling configured, and sized against the database's max connections?
29. Does anything need pagination, and is it offset-based (breaks at scale) or cursor-based?

## Integrity & consistency

30. What constraints are enforced by the database versus only by application code?
31. Are foreign keys actually declared, or just implied?
32. Are there uniqueness rules, and are they enforced with a unique index (not a check-then-insert)?
33. What's the transaction boundary for each multi-write operation?
34. Is eventual consistency acceptable anywhere, and does the UI reflect that honestly?
35. If two systems hold the same fact and disagree, which wins?

## Caching

36. What's cached, where, and for how long?
37. What invalidates the cache, and what's the worst-case staleness a user could see?
38. What happens on a cache miss storm (thundering herd) after a flush?
39. Is the cache a performance optimization or load-bearing? If it vanished, does the system still work?
40. Is anything user-specific cached in a shared cache by accident?

## Files & blobs

41. Where do uploaded files live — database, disk, object storage?
42. Are file names sanitized, and are user-supplied paths ever trusted?
43. Are files served directly or through signed URLs?
44. What's the retention policy, and who cleans up orphans?

## Backup & recovery

45. What's the backup frequency, and where are backups stored?
46. Has a restore actually been tested, or is it theoretical?
47. What's the acceptable data loss window (RPO) and recovery time (RTO)?
48. Is point-in-time recovery available if someone runs a bad UPDATE without a WHERE?
49. Is there an audit trail of who changed what?

## Data lifecycle

50. How long is data kept, and what triggers deletion?
51. Can a user's data be fully exported and fully deleted on request?
52. Is there PII in logs, analytics, or backups that the deletion path misses?
53. Is test/staging data anonymized, or is it a copy of production?

## Verification

- Run `EXPLAIN` on the top queries against a realistic dataset size.
- Test the down-migration on a copy of production-shaped data.
- Insert a duplicate that should be rejected — is it?
- Flush the cache under load and watch what happens.
- Perform an actual restore from backup into a scratch environment.
- Grep logs for anything resembling PII.
