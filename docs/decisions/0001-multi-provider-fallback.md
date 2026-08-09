# 0001 — Route across multiple model providers instead of committing to one

**Status:** Accepted and in production

---

## Context

An assistant that is always listening is only useful if it is always available. The
obvious architecture — pick the best model, call its API — makes the assistant exactly as
reliable as one vendor's uptime, rate limits, and billing.

In practice, single-provider dependency fails in four distinct ways, and only one of them
is an outage:

1. **Outage.** The provider is down. Rare, but total.
2. **Rate limiting.** Far more common. Indistinguishable from an outage to the user.
3. **Quota exhaustion.** Self-inflicted, and it always happens mid-task.
4. **Model deprecation.** A model identifier stops working on a schedule the vendor
   chooses.

Each of these turns a voice assistant into a device that does not answer when spoken to,
which is worse than not having one.

## Decision

Every model call goes through a **provider router** holding an ordered chain of providers.
On failure, the router walks the chain. Callers receive an answer without knowing which
provider produced it.

The chain ends in **locally hosted models**, so the final fallback requires no network at
all.

Two properties matter more than they look:

- **The chain is configuration, not code.** Reordering providers, or dropping one that has
  become expensive, is a config change.
- **Callers cannot observe the routing.** No feature branches on provider identity. This
  is what keeps the abstraction from leaking as the system grows.

## Alternatives considered

**Single provider, retry with backoff.** Simplest. Handles transient errors, does nothing
for outages, quota exhaustion, or deprecation — three of the four failure modes.

**Single provider, manual switch on failure.** Requires the user to notice and act, at
the exact moment they wanted an assistant to handle something for them.

**A third-party routing gateway.** Solves the problem and adds a new single point of
failure, plus a dependency holding the credentials for every provider.

**Local models only.** No vendor risk at all, and a permanent ceiling on quality. Rejected
as the primary path, adopted as the last link in the chain — where its guarantee of being
available matters more than its quality.

## Consequences

**Good**

- No vendor outage takes the assistant offline; quality degrades instead of availability.
- Cost control becomes a routing decision: cheap models first, expensive ones on fallback.
- Adopting a newly released model is a config edit, not a migration.
- The offline path is exercised continuously rather than being untested emergency code.

**Bad**

- Prompts must work acceptably across providers with different behaviours and tool-calling
  formats. This is a real, ongoing tax.
- Failures are quieter. A provider silently falling out of the chain looks like nothing at
  all, which makes monitoring the router's own behaviour necessary.
- Testing surface multiplies by the number of providers in the chain.

**Accepted trade-off**

Prompt portability costs real effort on every feature. It is still cheaper than an
assistant that is unavailable several times a month.
