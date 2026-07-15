# Setup — Codex CLI

Codex CLI reads repo instructions from an `AGENTS.md` file at startup and operates
inside a working directory, so it runs a crew cleanly. Two ways to do it.

## Option A — one repo, role-switching (simplest)

Best when you want a crew's *discipline* without running multiple terminals.

1. Put the crew files in your repo:
   ```
   ROSTER.md
   protocol/CROSS-AGENT.md
   crew/strategy/IDENTITY.md   crew/strategy/SOUL.md
   crew/content/IDENTITY.md    crew/content/SOUL.md
   ...
   ```
2. In your repo's **`AGENTS.md`** (Codex's startup-instructions file — its
   equivalent of `CLAUDE.md`), add:
   > This repo runs a crewkit crew. Before acting, read `ROSTER.md` to see who owns
   > what. When I tell you to act as a role (e.g. "as Content"), first read that
   > role's `crew/<role>/IDENTITY.md` + `SOUL.md` and `protocol/CROSS-AGENT.md`,
   > then stay strictly within that role's scope.
3. Drive it by naming the hat: *"As Strategy, rank this week's priorities."* Codex
   loads that role's files and acts in-scope. When work belongs to another role, it
   records the handoff (see the drop-box in step 3 of Option B) rather than silently
   doing it out of lane.

**Binding the primitives** (`protocol/CROSS-AGENT.md` §3–4):
- *Agent-to-agent send* → in single-session mode there's no separate agent to call,
  so a handoff is a **note you carry to the next role**: finish as Strategy, then
  start a turn "As Content, here's the angle Strategy set…". Keep the seam by not
  letting one hat do another's job.
- *Silence tokens* → mostly moot in one session; keep the discipline of not emitting
  filler ("noted, standing by") that adds nothing.

## Option B — separate Codex sessions per role (closer to a real crew)

Run each role as its own Codex session with its own working directory
(`crew/strategy/`, `crew/content/`, …), each carrying its `IDENTITY.md` + `SOUL.md`
+ the shared `protocol/CROSS-AGENT.md` (symlink the shared files so one edit
updates the whole crew). Point each session's `AGENTS.md` at *read IDENTITY.md,
SOUL.md, and CROSS-AGENT.md at start.*

**Binding the primitives:**
- *Agent-to-agent send* → Codex has no built-in cross-session messaging, so bind the
  handoff to a transport you control. The simplest that works: a shared
  **`handoffs.md` drop-box** — the requester appends a dated block ("→ research:
  verify X, needed for Y"), the receiving session reads it, does the work, and
  appends its reply below. A shared queue or an MCP tool works too if you have one.
- *The reply that wakes the requester* → with a file drop-box, "waking" is just the
  requester re-reading `handoffs.md` on its next turn. Keep replies in the file, not
  scattered across sessions, so the trail stays legible (this doubles as an audit log).
- *Silence tokens* → bind `REPLY_SKIP` / `ANNOUNCE_SKIP` to whatever your wrapper
  treats as "post nothing," so a role with nothing to add doesn't append noise to
  the drop-box.

## The anti-pattern to avoid

Don't let one role quietly do another role's work because it's faster than a
handoff. The whole point is that boundaries hold — a recorded handoff beats an
out-of-lane shortcut. (For Build especially: it must push *what to build* back to
Strategy, not decide it — see `crew/build/IDENTITY.md`.)

## Test the seam

Act as Content and ask for something that needs a fact only Research owns. Confirm:
the request routed to Research (via the hat-switch or the drop-box), Research
verified it, and the finished piece came back — without you hand-carrying the fact
yourself.

---

*Learning to build crews like this? Join **[AI Superpower · course-ai.app](https://course-ai.app/)** — we learn and build with AI together.*
