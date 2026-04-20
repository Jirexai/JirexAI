# Data layer

Persistent storage designed for the same properties the rest of the stack is designed for: bounded trust, exact arithmetic, no silent data loss, zero external dependencies on the critical path.

---

## Database engine — In Development

In-memory-first relational engine with write-ahead log durability, tree-structured indexing, and exact fractional arithmetic as a first-class type. Zero floating point. Zero null. Zero external dependencies.

**What it replaces.** For most JirexAI infrastructure use cases — configuration state, audit trails, event streams, small operational stores — SQLite, PostgreSQL, and MySQL bring orders of magnitude more surface than the workload requires, plus the supply chain cost of their extension ecosystems. The engine is sized for the workload, not the other way around.

**What it guarantees.**

- **Exact arithmetic.** `0.1 + 0.2 = 0.3` without caveat. Money columns never round; percentage columns never drift. The type system refuses to silently convert to floating point.
- **Explicit absence.** There is no `NULL`. A value is either present or is an optional-typed absence — and the query language cannot silently collapse them. Columns that want an "unknown" state declare it, with type support.
- **No sign confusion.** Balances, offsets, deltas — anything with direction — is represented as a positive magnitude plus a direction flag, in two columns. Double-entry bookkeeping as a type discipline.
- **Durability under crash.** Write-ahead log, fsync'd on every commit. Recovery is replay, not "hope".
- **Predictable query cost.** The query language does not have `SELECT *` equivalents that scan everything, and it does not have implicit joins that fan out. Bulk operations are explicit and audit-logged.

**Status.** Coded, under adversarial testing, not yet serving production traffic. The API surface is stabilizing through the current sprint cycle.

---

## Want depth?

For schema migration tooling, replication model, backup/restore semantics, benchmark comparisons, or commercial engagement, reach out via [jirex.ai](https://jirex.ai).
