# Setup — Claude Code

Claude Code reads markdown at startup and can spawn sub-agents, so it runs a crew
naturally. Two ways to do it.

## Option A — one project, sub-agents as the crew (simplest)

1. Put the crew files in your repo:
   ```
   ROSTER.md
   protocol/CROSS-AGENT.md
   .claude/agents/strategy.md      # role definition (IDENTITY + SOUL merged, or referenced)
   .claude/agents/content.md
   ...
   ```
2. In each `.claude/agents/<role>.md`, set the agent's system prompt to: *read
   `IDENTITY.md`, `SOUL.md`, and `protocol/CROSS-AGENT.md` for this role, then
   act within that scope.* (Or paste the role's IDENTITY/SOUL directly into the
   agent definition.)
3. Add a pointer in your root `CLAUDE.md`: *"This project runs a crewkit crew.
   Read `ROSTER.md` to see who owns what; delegate cross-role work to the owning
   sub-agent via the Agent tool."*

**Binding the primitives** (`protocol/CROSS-AGENT.md` §3–4):
- *Agent-to-agent send* → the **Agent tool** (`subagent_type: "<role>"`). It's
  synchronous — the sub-agent's final message returns to the caller as the tool
  result, which the caller synthesizes. That already gives you the invisible seam:
  sub-agent output is not shown to the user, only your synthesis is.
- *Silence tokens* → in a single-project setup the loop mostly self-manages, but
  keep the discipline: when a sub-agent has nothing to add, it returns a short
  "nothing to add" as its final message rather than inventing filler.

## Option B — separate Claude Code instances per role (closer to a real crew)

Run each role as its own Claude Code session with its own working directory
(`crew/strategy/`, `crew/content/`, …), each with its `IDENTITY.md` + `SOUL.md` +
the shared `protocol/CROSS-AGENT.md`. Bind agent-to-agent messaging to whatever
transport you wire between the sessions (a shared queue, files, an MCP tool). This
is the setup that most resembles the production crew crewkit was distilled from —
use `docs/setup-multichannel-runtime.md` as the reference implementation of that shape.

## Test the seam

Ask the content role for something needing a fact only research owns. Confirm one
human message in, one finished answer out, no relaying, and no sub-agent chatter
leaking to the user.
