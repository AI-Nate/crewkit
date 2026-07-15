# Setup — a multi-channel / multi-session runtime

This is the reference shape crewkit was distilled from: **one persistent agent per
channel (or workspace) = one role**, each with its own memory, coordinating over a
session-to-session message primitive. If your runtime runs several long-lived
agents that can message each other — many self-hosted and multi-agent chat
runtimes do — the mapping is direct.

## 1. One channel/workspace per role

Each role gets its own workspace holding its `IDENTITY.md` and `SOUL.md`. Put the
shared files (`protocol/CROSS-AGENT.md`, `ROSTER.md`) in one place and **symlink**
them into every workspace — edit once, the whole crew picks it up.

## 2. Read-at-startup

Point each agent's startup instructions at the three files: *"Read `IDENTITY.md`,
`SOUL.md`, and `CROSS-AGENT.md` at session start."*

## 3. Bind the primitives (`protocol/CROSS-AGENT.md` §3–4.1)

- **Agent-to-agent send** → your runtime's session-to-session send. Fire it
  **non-blocking** for anything slow (drafts, research, image gen, code): send,
  tell the human "on it," and let a later turn synthesize when the reply wakes you.
- **The reply that actually wakes the requester** → whatever your runtime treats as
  a *final* inter-agent reply, as opposed to a plain post into the receiver's own
  channel. On many multi-channel runtimes a plain message in a background/satellite
  session reaches no one — only a structured "final" hand-back wakes the caller.
  Verify which is which on yours before you rely on it.
- **Silence tokens** → bind `REPLY_SKIP` and `ANNOUNCE_SKIP` to your runtime's
  literal skip strings. This matters most here: multi-channel runtimes often mirror
  an agent's own output back as new input, so an invented meta-reply ("standing by")
  starts a real ping-pong loop. The literal tokens are the only safe silence.

## 4. The anti-pattern to block

Do **not** let one agent post into a sister's channel as a delegation shortcut. On
most such runtimes that post (a) is visible to the human, breaking the seam, and
(b) never wakes the sister, because inbound preflight tends to drop agent-authored
messages. Use the real session-to-session send every time. If your runtime doesn't
hard-block the shortcut for you, make it a crew rule.

## 5. Test the seam

Human asks the content role for something needing a fact research owns. Confirm:
one message in, one finished answer out, the handoff invisible, no self-echo loop.
