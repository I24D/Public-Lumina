# Design decisions

Architecture decision records for Lumina. One document per decision that shaped the
system, written when the decision was made rather than reconstructed afterwards.

Each record states the problem, the choice, the alternatives that were rejected and why,
and the consequences — including the bad ones. A decision record that lists only benefits
is marketing, not engineering.

| # | Decision | Status |
|---|---|---|
| [0001](0001-multi-provider-fallback.md) | Route across multiple model providers instead of committing to one | Accepted |
| [0002](0002-duplex-voice-barge-in.md) | Build a custom voice activity gate instead of using push-to-talk | Accepted |
| [0003](0003-pgvector-over-dedicated-vector-db.md) | Store embeddings in PostgreSQL rather than a dedicated vector database | Accepted |
| [0004](0004-ui-automation-over-apis.md) | Automate the desktop through UI Automation instead of per-application APIs | Accepted |

## Format

```
# NNNN — Decision, stated as the choice made

Status: Proposed | Accepted | Superseded by NNNN

## Context      — the problem, and why the obvious answer fails
## Decision     — what was chosen
## Alternatives — what was rejected, and the specific reason
## Consequences — good, bad, and the trade-off explicitly accepted
```

Superseded records are kept rather than deleted. Being able to see which decisions were
reversed, and why, is more useful than a tidy list of the ones that survived.
