---
name: "writing"
description: "Assists with writing and editing general prose using Strunk & White's Elements of Style and Wikipedia's AI writing detection guide. Use when composing articles, blog posts, or editing text for clarity and authenticity."
---

# Writing and Editing

Clear prose, no AI tells.

## When to Use This Skill

**Use for:**
- Drafting or editing **prose** (articles, essays, blog posts)
- User explicitly requests style guidance or "Elements of Style"
- Reviewing text for AI writing patterns

**Do NOT use for:**
- Code comments, docstrings, or inline documentation
- Commit messages or changelogs
- Technical docs (prefer terse/direct for those)
- README files (different conventions)
- Chat messages or quick replies

## Quick Start

**Always load first:**
```
Read style-guides/quick-ref.md
```

This gives you the top rules and AI checklist in ~60 lines. Load full guides only when needed.

## Task Routing

| Task | Load | Why |
|------|------|-----|
| **Draft from scratch** | `quick-ref.md` then `elements-of-style.md` | Composition rules for structure |
| **Edit existing prose** | `quick-ref.md` then both full guides | Need usage rules + AI patterns |
| **Final AI audit** | `quick-ref.md` then `ai-writing-patterns.md` | Focus on detection/removal |
| **Grammar question** | `elements-of-style.md` directly | Usage rules in Part II |
| **Quick review** | `quick-ref.md` only | Checklist is often enough |

## File Structure

```
style-guides/
├── quick-ref.md           # ~60 lines, load first always
├── elements-of-style.md   # Full Strunk & White (deep editing)
└── ai-writing-patterns.md # Distilled AI tells (final review)
```

## Workflow

### Before Writing
1. Load `quick-ref.md`
2. Identify thesis/main point
3. Outline structure (one topic per paragraph)

### During Drafting
1. Write in prose (no bullets unless requested)
2. Active voice, concrete language
3. One paragraph per topic, topic sentence first

### During Editing
1. Apply composition rules (10-13, 18)
2. Check mechanics (5, 7)
3. Run AI checklist from quick-ref

### Final Review
1. Scan for red-flag phrases
2. Check sentence endings for -ing analysis
3. Verify significance claims have facts
4. Read aloud — does it sound human?

## Critical Rules

1. **Write prose** — Bullets only if explicitly requested
2. **Concrete > abstract** — Facts over significance claims
3. **Active voice** — Passive weakens
4. **Every word earns its place** — Omit needless words
5. **Check AI patterns** — Always, even on human drafts

## Examples

### Quick edit (most common)
```
1. Read style-guides/quick-ref.md
2. Apply checklist
3. Done
```

### Deep drafting session
```
1. Read style-guides/quick-ref.md
2. Read style-guides/elements-of-style.md (composition section)
3. Draft
4. Read style-guides/ai-writing-patterns.md
5. Final audit
```

### Investigating a specific rule
```
Read style-guides/elements-of-style.md
Search for the rule number or keyword
```

## Notes

- `quick-ref.md` is the 80/20 — covers most cases
- `elements-of-style.md` is canonical for grammar disputes
- `ai-writing-patterns.md` is prescriptive — follow it strictly
