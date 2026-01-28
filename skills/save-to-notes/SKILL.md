---
name: save-to-notes
description: Save content to Obsidian vault inbox for later processing. Use when the user says "save to my notes", "save to vault", "save to Obsidian", "save to my inbox" or asks to save conversation output (research, recommendations, plans, etc.) to their notes.
---

# Save to Vault

Write content to the Obsidian vault's `00_Inbox/` folder.

## Vault Path

```
~/Library/Mobile Documents/iCloud~md~obsidian/Documents/Notes/00_Inbox/
```

## Process

1. **Determine content**: Use provided content, or summarize/format the relevant conversation output (research, recommendations, plans, etc.)
2. **Generate filename**: `YYYY-MM-DD - [Title].md` using today's date; clean title (remove special chars, keep concise)
3. **Write note** to `00_Inbox/` using the structure below

## Note Structure

```markdown
---
created: YYYY-MM-DD
tags:
  - needs-processing
status: inbox
source: [Claude|ChatGPT|Gemini|etc]
---

# [Title]

[Content formatted with headers, lists as appropriate]

---
*Saved from conversation on YYYY-MM-DD*
```

## Guidelines

- Keep formatting clean and scannable
- For research: include sources/links
- For recommendations: include reasoning
- For plans: include actionable next steps
