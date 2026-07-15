# Setup — any markdown agent

crewkit needs only two things from your runtime:

1. **Each agent reads files at startup.** So it can load `IDENTITY.md`, `SOUL.md`,
   and `protocol/CROSS-AGENT.md` before it acts.
2. **One agent can send a message to another.** So handoffs work without the human.

If your framework does both (Cursor, Codex CLI, aider with a wrapper, a homegrown
loop, n8n/LangGraph nodes, …), it can run a crew. Here's the generic binding.

## 1. One workspace per role

Give each role its own working directory:

```
crew/
  ROSTER.md                 # copied from templates/ROSTER.template.md, filled in
  protocol/CROSS-AGENT.md   # copied (or symlinked) — same file for every role
  strategy/
    IDENTITY.md
    SOUL.md
  content/
    IDENTITY.md
    SOUL.md
  ...
```

Symlink `protocol/CROSS-AGENT.md` and `ROSTER.md` into each role dir if you can —
then one edit updates the whole crew.

## 2. Make each agent read its three files at startup

Whatever your "system prompt" or "startup instructions" hook is, add:

> At session start, read `IDENTITY.md` (what you own), `SOUL.md` (how you behave),
> and `protocol/CROSS-AGENT.md` (how the crew coordinates). Follow them.

## 3. Bind the two primitives

In `protocol/CROSS-AGENT.md`, two things are left abstract. Bind them here for
your runtime:

- **"Your framework's agent-to-agent send"** → the actual call/function/node that
  delivers a message to another agent and gets you its reply. Note whether it's
  blocking or fire-and-forget, and how the reply comes back (return value,
  callback, a new inbound message).
- **The silence tokens** (`REPLY_SKIP` / `ANNOUNCE_SKIP`) → whatever string or
  mechanism makes *your* runtime post nothing. If your runtime already has a true
  no-op reply, use that and note it. If it posts everything you output (most do),
  define the exact literal tokens your loop treats as "emit nothing."

## 4. Test the seam

Ask role A for something that needs a fact from role B. Confirm: the human sent
one message, role A asked role B directly, and the human got one finished answer
without relaying. If B's reply posted somewhere visible, or never woke A, fix the
binding in step 3 before you add more roles.
