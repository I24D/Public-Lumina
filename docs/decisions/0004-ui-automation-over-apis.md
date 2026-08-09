# 0004 — Automate the desktop through UI Automation instead of per-application APIs

**Status:** Accepted and in production

---

## Context

For the assistant to be useful on a desktop, it has to operate the applications already
there — read what is on screen, act on notifications, reply in a messaging client. The
question is how it reaches into software it does not own.

Every option is unsatisfying in a different way. Official APIs exist for some
applications and not others, and rarely cover the desktop client specifically. Many
consumer applications have no automation surface at all, by design.

## Decision

Desktop control goes through the operating system's **UI Automation** accessibility layer:
the same interface screen readers use to expose window contents and controls.

Applications are driven the way a user drives them — by locating controls and interacting
with them — rather than through bespoke integrations per application.

A practical detail that shaped the implementation: the automation layer is driven from
**Python** rather than PowerShell, because endpoint security software routinely flags
scripted UI automation from a shell as suspicious behaviour and blocks it. This is not a
theoretical concern; it was discovered by being blocked.

## Alternatives considered

**Official APIs per application.** Best when they exist. They do not exist for most
desktop clients, coverage is partial where they do, and each one is a separate
integration, a separate credential, and a separate breaking-change schedule.

**Screen scraping with OCR plus synthetic clicks.** Universal, and fragile in every
direction: resolution, theme, font scaling, and window position all break it. UI
Automation exposes the actual control tree, which is strictly more information than
pixels.

**Browser automation for web versions.** Works where a web client exists, and means
running and maintaining a separate browser session per application. Reasonable as a
supplement, insufficient as the foundation.

**Injecting into application processes.** Most capable and least defensible. Breaks on
every update, is indistinguishable from malware to security software, and violates the
terms of most applications.

## Consequences

**Good**

- One mechanism reaches every accessible application, including those with no API at all.
- No per-application credentials, and no dependency on a vendor continuing to offer
  automation.
- Because it uses the accessibility layer, it degrades in the same direction accessibility
  does: applications that work with screen readers work here.

**Bad**

- Slower than an API. Traversing a control tree costs more than an HTTP call.
- Vulnerable to interface redesigns. An application that moves its controls breaks the
  automation, and this happens without warning.
- Requires an interactive desktop session; it cannot run headless.
- Sits in territory endpoint security treats as suspicious, which constrains
  implementation choices.

**Accepted trade-off**

Fragility across application updates is accepted in exchange for reach. An assistant that
works with every application imperfectly is more useful than one that works with three
applications perfectly.
