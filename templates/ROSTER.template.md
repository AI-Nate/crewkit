# ROSTER.md — the crew's org chart

_The one-page map of who owns what. This is the source of truth for routing: when
a task lands, an agent checks here to decide "is this mine, or whose is it?"
Symlink or copy it where every agent can read it._

## The one rule

**Every domain has exactly one owner. No domain has zero owners.** If a piece of
work doesn't clearly belong to exactly one role, the roster is wrong — fix the
roster, not the prompt. Crews grow by *boundary pressure*: add a role only when
you can name a domain no existing role should own.

## Roster

| Role | Owns (one-sentence boundary) | Does NOT own (→ owner) |
|---|---|---|
| <Role A> | <the domain it defends> | <adjacent domain → Role B> |
| <Role B> | <...> | <...> |
| <Role C> | <...> | <...> |

## Escalate to the human when

_The short list of calls no agent makes alone — the seam becomes visible here on purpose._

- <a pricing / strategy / legal / brand call only the human owns>
- <any irreversible external action: publishing, spending, sending to an audience>
- <a contradiction between two roles that they can't resolve between themselves>

## Handoff quick-reference

- Need something a sister owns? Send it to them directly (see `protocol/CROSS-AGENT.md` §2).
- Nothing useful to say? Emit a silence token — never a meta-reply (§4).
- Load-bearing fact? Verify before shipping (§2.1).
