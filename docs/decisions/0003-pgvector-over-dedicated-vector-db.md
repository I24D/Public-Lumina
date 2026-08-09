# 0003 — Store embeddings in PostgreSQL rather than a dedicated vector database

**Status:** Accepted and in production

---

## Context

Long-term memory needs semantic search: given what the user just said, retrieve the
relevant things they said weeks ago. That means embeddings and nearest-neighbour search.

The default advice is to reach for a purpose-built vector database. But memory is not only
vectors. A recalled fact has an owner, a timestamp, a source conversation, a confidence,
and relationships to other facts. Retrieval is rarely pure similarity — it is similarity
*filtered by* recency, subject, or source.

Splitting that across two stores means every read joins across a network boundary, and
every write has to keep two systems consistent with no transaction spanning them.

## Decision

Embeddings live in **PostgreSQL** alongside the relational data they belong to, using the
`pgvector` extension for similarity search.

One store. One transaction. Vector similarity and relational filters compose in a single
query, because they are columns in the same table.

## Alternatives considered

**A dedicated vector database.** Faster at very large scale and better at pure vector
workloads. It also introduces a second store to operate, a second consistency problem,
and — decisively — pushes metadata filtering either into the vector store's limited
filtering or into the application layer, where it becomes fetch-then-filter.

**In-memory index rebuilt at startup.** Fast and simple until memory outgrows RAM or the
process restarts mid-session. Rejected because durability was the entire point of building
memory.

**Flat files with brute-force similarity.** Genuinely adequate at small scale, and the
honest answer for a prototype. Rejected because the growth curve is unfavourable and
migrating later would mean rewriting retrieval.

## Consequences

**Good**

- One database to back up, migrate, and reason about.
- Similarity search and metadata filtering in a single query, with the planner deciding
  the strategy — not the application fetching candidates and filtering them in code.
- Writes to a fact and its embedding are one transaction. There is no state where they
  disagree.
- Standard PostgreSQL tooling applies to the memory system.

**Bad**

- At very large scale, a specialised vector store will outperform this. That ceiling is
  real, it is simply far above single-user workloads.
- Index tuning for vector columns is less mature than for conventional indexes.
- Ties the memory layer to PostgreSQL specifically, not merely to "a database".

**Accepted trade-off**

This is a single-user, local-first system. Choosing the architecture that wins at a scale
this product will never reach would mean paying its operational cost every day in exchange
for a benefit that never arrives.
