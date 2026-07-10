---
name: summary-generator
description: >-
  Automatically creates a downloadable Markdown handoff summary of the
  conversation so the user can paste it into a new chat or different AI
  model and continue without losing progress. There's no way to read an
  exact token percentage, so trigger this proactively and WITHOUT asking
  permission when: a long_conversation_reminder appears, the conversation
  has clearly grown very long or heavy (dozens of turns, several large
  artifacts, an extended multi-step task), or the user hints they're
  worried about losing the chat or running low on room. Also trigger on
  direct requests like "summarize this chat", "give me a handoff/recap",
  "I need to continue this in ChatGPT/Gemini/a new chat", "export this
  conversation", or "save my progress". Generate and deliver the file as
  part of the normal reply -- don't ask first -- and briefly explain why.
  Don't regenerate every turn; only make a new one after substantial new
  work or a fresh explicit request.
---

# Summary Generator

Creates a self-contained, portable summary of the current conversation and saves it as a downloadable Markdown file — so the work can continue in a new chat, with Claude or any other AI model, without repeating context or re-deciding things that are already settled.

## Why the trigger works this way

There's no tool or telemetry that reports "context is at 90%" as a literal number — skills don't watch a token counter, they get consulted when a request or situation matches this description. So instead of one precise threshold, treat these as sufficient signals to act on **immediately, without asking permission first**:

1. A `long_conversation_reminder` is appended to the user's message — the clearest signal available that Anthropic itself considers the conversation long.
2. The conversation has obviously grown large by other signs: dozens of turns, several large code files or documents produced, or many rounds spent on one complex task.
3. The user hints they're worried about losing the thread — "this is getting long," "I don't want to lose this," "can you save where we're at."
4. The user directly asks for a summary, recap, handoff, export, or to continue elsewhere.

When any of these show up, generate and deliver the file as part of the normal reply — don't ask "want me to do this?" first, since that defeats the point of catching it early. Still fully answer whatever the user actually asked in that turn; the summary rides along with the real answer, it doesn't replace it. Add one plain sentence explaining why the file showed up, e.g. "This thread's gotten pretty long, so I put together a handoff file in case you want to continue elsewhere" — so it doesn't feel random.

Once a summary has been generated for this conversation, don't make another one on every subsequent turn. Only generate a new one if a lot of substantial work has happened since the last one, or the user explicitly asks again.

## Gathering the content

Reread the whole conversation, not just the last few messages. Pull out:

- **The actual goal(s)** the user is working toward
- **Constraints and facts that matter for continuing correctly** — stated preferences, technical requirements, names/paths/values given along the way
- **Decisions that took real back-and-forth to land on** — these are the most expensive thing to lose, since re-deriving them wastes the most time for whoever picks this up
- **Approaches already tried and rejected**, and briefly why, so they don't get proposed again
- **The current state of any artifacts** — code, documents, plans — what's finished, what's half-built, what's still just an idea
- **What's left to do**

Leave out pleasantries, throat-clearing, and a turn-by-turn transcript. Someone reading this — human or model — should be able to act on it in under a minute, not have to mine it for the relevant parts.

## Output format

Save the file as Markdown and deliver it as a download. Use this structure, but adapt it to what's actually in the conversation — drop sections that have nothing to say rather than writing "N/A":

```markdown
# Conversation Handoff Summary

I'm continuing work from a previous AI conversation that got too long to keep going. Here's the full context so far — please read it and pick up from here instead of starting over.

## Goal
[what the user is trying to accomplish]

## Key context & constraints
[facts, preferences, values, names/paths, anything stated that matters]

## Decisions already made — please don't re-litigate these
[decision — brief reasoning, for each one]

## Current state
[what exists right now, and what's done vs. still in progress]

## Already tried and rejected
[approach — why it didn't work, if relevant]

## Next steps
[what still needs to happen, roughly in order]
```

Keep every section tight — bullets over paragraphs, specifics over summaries-of-summaries. Favor completeness of the decisions and current-state sections over the others; those are what save the most time on the other end.

## Filename

Save as `handoff-summary-<short-topic-slug>.md` — e.g. `handoff-summary-react-dashboard.md`. If a second one gets made later in the same conversation, suffix it `-2`, `-3`, and so on rather than overwriting the first.
