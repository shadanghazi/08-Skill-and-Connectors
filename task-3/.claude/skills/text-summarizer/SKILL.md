---
name: text-summarizer
description: >-
  Summarizes whatever text the user pastes, types, or dictates into a
  short paragraph and saves it as a downloadable Markdown file. Use this
  skill whenever the user explicitly asks to "summarize this", "summarize
  what I just said/pasted", "give me a summary of this text", or similar
  direct requests to condense a specific piece of content they've
  provided in the conversation. Do NOT trigger this automatically just
  because a message is long -- only use it when the user explicitly asks
  for a summary of something they said or pasted.
---

# Text Summarizer

Condenses a piece of text the user has provided (pasted, typed, or dictated earlier in the conversation) into a short paragraph, and delivers it as a downloadable file.

## When to use this

Only trigger on an explicit ask, e.g.:
- "Summarize this: [text]"
- "Can you summarize what I just said?"
- "Summarize that paste above"
- "Give me a summary of this"

Don't trigger just because a message in the conversation is long — wait for the direct request.

## Identifying the source text

- If the user pastes text in the same message as the request, summarize that.
- If they refer to something said earlier ("summarize what I just said", "summarize that"), use the most recent substantial chunk of user-provided text in the conversation — not Claude's own prior replies, unless the user clearly means those instead.
- If it's genuinely ambiguous what they want summarized (e.g. nothing text-like appears nearby), ask a single clarifying question rather than guessing.

## Writing the summary

- One short paragraph (roughly 3-6 sentences, fewer for short source text). No bullet points, no headers within the summary itself.
- Capture the core point(s) and any conclusions or decisions in the source text — not a blow-by-blow retelling.
- Write in plain prose, in Claude's own words. Never quote the source verbatim beyond a few words here and there.
- Match the general subject-matter tone (neutral/factual for factual text, etc.) but keep the summary itself plain and clear.

## Output

Save the summary as a Markdown file and deliver it as a download alongside a normal chat reply (don't just print the summary in the free text response as the answer). The chat reply itself should also contain the summary text so the user sees it immediately, not only in the file.

Filename: `summary-<short-topic-slug>.md`, e.g. `summary-quarterly-report.md`.

File content:

```markdown
# Summary

[the short paragraph summary]
```

If the user asks for repeated summaries in one conversation, use `-2`, `-3`, etc. suffixes rather than overwriting the previous file.
