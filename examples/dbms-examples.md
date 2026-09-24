# Example — DBMS (Unit: Normalization + Transactions)

Subject-adaptability proof. Same rules, different subject.

---

# DBMS — OpenBook Reference Pack
Units: Normalization, Transactions
Exam: Midterm (Open Book) | Page budget: 8 | Estimated: 7.6

---

## Table of Contents

- §1.1 Functional Dependencies
- §1.2 Normal Forms (1NF–BCNF)
- §1.3 Decomposition
- §2.1 ACID Properties
- §2.2 Concurrency Control
- §2.3 Deadlock
- §3 HOTS — Both Units
- Quick Revision Sheet
- Index

---

## Unit 1 — Normalization

### §1.1 Functional Dependencies  [S1]

A functional dependency X → Y means: for any two tuples with the same X value, the Y value must also be the same. FDs are the foundation of normalization.

| Term | Meaning |
|---|---|
| Trivial FD | Y ⊆ X |
| Non-trivial FD | Y ⊄ X |
| Full FD | No proper subset of X determines Y |
| Partial FD | A proper subset of X determines Y |
| Transitive FD | X → Y and Y → Z implies X → Z |

> Exam relevance: Always asked as "find candidate key given FDs" or "classify FD types".

---

### §1.2 Normal Forms (1NF–BCNF)  [S1]

| Form | Condition | Removes |
|---|---|---|
| 1NF | Atomic values only | Multi-valued attributes |
| 2NF | 1NF + no partial dependency | Partial dependency on composite key |
| 3NF | 2NF + no transitive dependency | Transitive dependency |
| BCNF | For every FD X → Y, X is a superkey | All FD-based anomalies |

**Procedure — check normal form**

1. Identify all candidate keys.
2. Find all FDs.
3. Check 1NF: any multi-valued attribute? No → 1NF.
4. Check 2NF: any non-prime attribute dependent on part of a key? No → 2NF.
5. Check 3NF: any non-prime attribute dependent on another non-prime? No → 3NF.
6. Check BCNF: for every FD, is LHS a superkey? Yes → BCNF.

Edge cases: BCNF decomposition may not be dependency-preserving.

> Exam relevance: Given a relation + FDs, "normalize to 3NF/BCNF" is a standard 10-mark question.

---

### §1.3 Decomposition  [S1]

**Procedure — lossless join decomposition**

1. Compute attribute closure of each FD.
2. Find a decomposition where R1 ∩ R2 is a superkey of at least one side.
3. Verify no tuples are lost on natural join.

**Procedure — dependency-preserving decomposition**

1. Compute minimal cover Fc of F.
2. For each FD in Fc, create a relation with its attributes.
3. If no relation contains a candidate key, add one.

Edge cases: A decomposition can be lossless but not dependency-preserving (classic 3NF vs BCNF tradeoff).

---

## Unit 2 — Transactions

### §2.1 ACID Properties  [S2]

A transaction is a logical unit of work. ACID properties guarantee correctness despite failures and concurrency.

| Property | Meaning | Ensured by |
|---|---|---|
| Atomicity | All or nothing | Undo log / rollback |
| Consistency | DB moves between valid states | Application + constraints |
| Isolation | Concurrent txns don't interfere | Concurrency control |
| Durability | Committed changes survive crashes | Redo log / WAL |

> Exam relevance: "Define ACID with an example" is a recurring 5-mark question.

---

### §2.2 Concurrency Control  [S2]

**Procedure — two-phase locking (2PL)**

1. Growing phase: acquire locks, no release.
2. Shrinking phase: release locks, no acquire.
3. Serializability guaranteed if all txns follow 2PL.

Variants:

| Variant | Difference |
|---|---|
| Strict 2PL | Hold all locks until commit/abort |
| Rigorous 2PL | Hold all locks until commit |
| Conservative 2PL | Acquire all locks before starting |

Edge cases: 2PL guarantees serializability but not deadlock-freedom.

---

### §2.3 Deadlock  [S2]

**Step-trace — deadlock cycle**

    Step 1: T1 acquires lock on A
    Step 2: T2 acquires lock on B
    Step 3: T1 requests lock on B — waits
    Step 4: T2 requests lock on A — waits
    Step 5: Cycle formed — deadlock

    Final answer: Deadlock occurs when a wait-for cycle exists among transactions.

**Prevention techniques**

- Wait-die: older txn waits, younger aborts
- Wound-wait: older txn wounds (aborts) younger
- Timeout: abort if wait exceeds threshold
- Detection: build wait-for graph, check cycle

---

## §3 HOTS — Both Units

**Q1 [Conceptual]** Why is BCNF not always achievable with a dependency-preserving decomposition?

**A:** BCNF requires the LHS of every FD to be a superkey. Some schemas have FDs whose LHS is not a superkey but is essential for preserving the FD. Splitting such an FD into BCNF may require decomposing into relations that lose the FD. Example: R(A, B, C) with FDs AB → C and C → B. BCNF decomposition is (A, C) and (B, C), which loses AB → C. Tradeoff: 3NF preserves dependencies; BCNF removes all anomalies. Practical DBMS often accept 3NF.

**Q2 [Application]** Given R(A, B, C, D) and FDs {AB → C, C → D, D → A}, normalize to BCNF.

**A:**

    Step 1: Compute candidate keys: AB, BC, BD
    Step 2: Check AB → C: AB is a superkey — OK
    Step 3: Check C → D: C is not a superkey — violates BCNF
    Step 4: Decompose R into R1(C, D) and R2(A, B, C)
    Step 5: Check R1: C → D holds, C is key — BCNF
    Step 6: Check R2: AB → C holds, AB is key — BCNF

    Final answer: R1(C, D), R2(A, B, C). Lossless but AB → C is not preserved in a single relation.

**Q3 [Trace/Output]** Two transactions T1 and T2 execute under strict 2PL. T1 locks A, T2 locks B, T1 requests B, T2 requests A. What happens?

**A:**

    Step 1: T1 holds lock on A, waits for B
    Step 2: T2 holds lock on B, waits for A
    Step 3: Neither releases — cycle forms
    Step 4: 2PL does NOT prevent deadlock; it only guarantees serializability

    Final answer: Deadlock. Both transactions wait indefinitely unless a deadlock detection or prevention scheme is active.

**Q4 [Design]** Design a locking scheme for a banking system where transfers must be atomic and deadlock-free.

**A:** Use strict 2PL with **ordered locking**: always acquire locks in a fixed global order (e.g., by account ID ascending). This prevents cyclic waits, so deadlock cannot form. Combine with:
- Short transactions (minimize lock hold time)
- Timeout-based abort as fallback
- Write-ahead logging for durability

---

## Quick Revision Sheet

### Unit 1 — Normalization

- FD X → Y: same X implies same Y.
- 1NF: atomic; 2NF: no partial; 3NF: no transitive; BCNF: LHS is superkey.
- Lossless decomposition: R1 ∩ R2 is superkey of one side.
- Dependency-preserving decomposition: every FD in minimal cover survives.
- 3NF preserves dependencies; BCNF removes all FD anomalies. Tradeoff.

### Unit 2 — Transactions

- ACID = Atomicity, Consistency, Isolation, Durability.
- 2PL: growing + shrinking phase; guarantees serializability.
- Strict 2PL: hold locks until commit/abort.
- Deadlock = wait-for cycle; prevent via wait-die, wound-wait, or ordered locking.
- WAL ensures durability; undo log ensures atomicity.

---

## Index / Cross-References

| Concept | Section |
|---|---|
| Functional dependency | §1.1 |
| Trivial / non-trivial FD | §1.1 |
| 1NF, 2NF, 3NF, BCNF | §1.2 |
| Lossless decomposition | §1.3 |
| Dependency-preserving | §1.3 |
| ACID properties | §2.1 |
| 2PL | §2.2 |
| Strict 2PL | §2.2 |
| Deadlock | §2.3 |
| Wait-die / wound-wait | §2.3 |