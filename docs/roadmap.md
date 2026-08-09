# Roadmap

Where Lumina is going, and what is deliberately not being built.

---

## Working today

- Multi-provider model routing with ordered fallback to local models
- Duplex voice with barge-in — interruption mid-response
- Long-term semantic memory with retrieval across sessions
- Desktop automation through UI Automation: reading windows, driving controls,
  capturing and replying to notifications
- Agent tool layer covering messaging, email, web search, and media generation
- MCP server exposing the platform to external AI clients
- Three client surfaces on one core: voice, VS Code extension, desktop orb

## In progress

- **Extracting reusable libraries.** Pulling the genuinely general parts of the platform
  out as standalone open-source packages — the provider router, the voice activity gate,
  the UI Automation helpers. See
  [what is public and what is not](../README.md#what-is-public-and-what-is-not).
- **Architecture documentation.** This repository. Ongoing.
- **Memory quality.** Retrieval currently favours recall over precision. Bringing the
  wrong context into a prompt is worse than bringing none.

## Considered, not committed

- Cross-platform support beyond Windows. The desktop automation layer is the obstacle;
  the rest of the system is portable.
- Multi-user operation. Would require rethinking the security model from a local-first,
  single-user assumption. Large change, unclear benefit for this product.
- A hosted version. Directly at odds with the design constraint that the assistant runs
  on the user's own machine with access to it.

## Explicitly not planned

- **Training or fine-tuning models.** This is an orchestration and integration project.
  Model capability is treated as a commodity input that improves on someone else's
  schedule.
- **A plugin marketplace.** Tool definitions are deliberately simple. An ecosystem would
  mean a stability guarantee that would slow the platform down at exactly the wrong stage.
- **Mobile clients.** The product's value comes from operating a desktop. A phone client
  would be a chat app with extra steps.

---

*This roadmap describes intent, not commitments. Items move between sections as the
project learns things.*
