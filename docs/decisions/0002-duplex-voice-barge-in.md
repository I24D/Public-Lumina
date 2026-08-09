# 0002 — Build a custom voice activity gate instead of using push-to-talk

**Status:** Accepted and in production

---

## Context

Voice assistants generally solve the "when is the user talking?" problem by avoiding it:
a wake word, or a button held down while speaking. Both work. Both also make the
interface worse in the specific way that matters — you cannot interrupt.

Human conversation is full of interruption. You cut in when the other person has
misunderstood you, or has said enough. An assistant that must finish its sentence before
hearing you is not conversational; it is a speaker with a queue. And when responses are
long — reading a document, reporting a result — the inability to interrupt stops being a
nuisance and becomes the reason the feature goes unused.

Allowing interruption creates a harder problem: the assistant's own voice comes back
through the microphone. Naive detection hears that as user speech, decides it has been
interrupted, and stops talking — every time it talks. The assistant silences itself.

## Decision

A **voice activity gate** sits between the microphone and the agent, and is aware of the
system's own output state. It distinguishes three things a naive detector conflates:

1. User speech while the assistant is silent → normal input.
2. User speech while the assistant is speaking → a genuine interruption; stop and listen.
3. The assistant's own audio returning through the room → ignore entirely.

The gate is duplex-aware: it knows what the system is currently playing, and uses that to
interpret what it hears. Interruption is therefore a supported state rather than an
accident.

## Alternatives considered

**Push-to-talk.** Trivial, reliable, and rules out an always-listening assistant — which
was the product. Also unusable hands-free, which is the main reason to build voice at all.

**Wake word before every utterance.** Works, but forces a ritual before each turn and
still does not permit interruption mid-response.

**Half-duplex — deafen the microphone while speaking.** The standard fix, and the reason
so many voice assistants cannot be interrupted. Eliminates echo by eliminating the
capability.

**Off-the-shelf echo cancellation.** Helps and is used, but tuned for telephony, not for
an assistant that must decide whether being talked over is meaningful. It answers "is
there echo?", not "did the user mean to interrupt me?".

## Consequences

**Good**

- Interruption works mid-response, which makes long outputs safe to attempt.
- Hands-free operation with no wake word and no button.
- Conversation feels like conversation rather than turn-taking with a machine.

**Bad**

- The gate is genuinely difficult to get right and is sensitive to room acoustics,
  microphone placement, and speaker volume.
- Failures are highly visible: a gate that is too eager cuts the assistant off constantly;
  one too conservative ignores real interruptions.
- It is stateful, which makes it harder to test than a pure function would be.

**Accepted trade-off**

This is the most difficult component in the system relative to its size, and it is worth
it. Interruption is the single feature that most separates a voice interface people
actually use from one they abandon after a week.
