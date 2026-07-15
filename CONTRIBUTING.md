# Contributing

The most valuable contribution is a **new example crew** — a role roster for a
domain we don't cover yet (a solo law practice, an indie game studio, a research
lab, a two-person agency). Drop it in `examples/<your-crew>/`.

## Ground rules

1. **Roles must own a domain, not a task.** A good role has a boundary you could
   defend in one sentence ("I own everything that ships to a customer's inbox").
   "Helper agent" or "does misc stuff" gets your PR closed with love — that's the
   anti-pattern the whole kit exists to prevent.
2. **IDENTITY answers *what I own*; SOUL answers *how I behave*.** Keep the split.
   If a rule is about scope/boundaries it goes in IDENTITY; if it's about voice,
   taste, or working style it goes in SOUL. Mixing them is the most common mistake.
3. **One worked handoff required.** Every example crew ships with at least one
   worked cross-agent handoff in its README, showing the seam staying invisible.
4. **No real names, clients, metrics, or credentials.** All examples are
   fictional. PRs containing anything resembling a real client, an API key, a
   session token, or a private number are rejected.
5. **Harness-agnostic.** The protocol describes *patterns*, not one framework's
   tool names. If your contribution hard-codes a specific runtime's API, move it
   into `docs/setup-<harness>.md` instead of the core protocol.

## Non-example contributions

Fixes to the templates, the protocol, or the setup guides are welcome. Keep the
core idea unchanged — **role-owned agents + an invisible-seam coordination
protocol + disciplined silence** — that's the product. Translations welcome.
