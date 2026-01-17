**INVARIANTS.md**

Rules that break the product if violated. Not best practices — actual failure modes.

---

**1. Tenant isolation is enforced at DB level via RLS.**
If tenant A sees tenant B's data, even once, the product is dead. No query-level filtering — RLS policies on every tenant-scoped table, context set in middleware.

**2. Entries are append-only.**
Never UPDATE or DELETE an entry. Corrections = reversal entry + new entry. This is the entire product promise — if history can be rewritten, they'll stay in Excel.

**3. Two-sided transactions are atomic.**
Both entries (investment + cash account) with shared `transaction_id` written in one DB transaction. Partial write = corrupted ledger.

**4. CSV cells starting with `=`, `+`, `-`, `@` are rejected.**
Formula injection. Family offices are high-value targets. Trivial to prevent, catastrophic if exploited.