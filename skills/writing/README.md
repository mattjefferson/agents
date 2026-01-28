# Claude Code Writing Skill

Clear prose, no AI tells.

## Overview

This skill helps Claude assist with drafting and editing prose. It provides:

1. **Quick Reference** — 60-line summary with top rules + AI checklist
2. **Elements of Style** — Full Strunk & White for deep editing
3. **AI Writing Patterns** — Distilled guide to detecting/removing AI tells

## Installation

```bash
# Clone to skills directory
git clone https://github.com/edwinhu/claude-code-writing-skill.git ~/.claude/skills/writing
```

## File Structure

```
style-guides/
├── quick-ref.md           # Load first — top rules + checklist
├── elements-of-style.md   # Full Strunk & White
└── ai-writing-patterns.md # Distilled AI detection guide
```

## Usage

### Quick Start

Most tasks need only the quick reference:

```
Load style-guides/quick-ref.md
Apply checklist
Done
```

### Task Routing

| Task | Load |
|------|------|
| Quick review | `quick-ref.md` only |
| Draft from scratch | `quick-ref.md` → `elements-of-style.md` |
| Edit existing | `quick-ref.md` → both full guides |
| Final AI audit | `quick-ref.md` → `ai-writing-patterns.md` |
| Grammar question | `elements-of-style.md` directly |

### When to Use

**Use for:** Prose — articles, essays, blog posts, long-form writing

**Skip for:** Code comments, commit messages, technical docs, READMEs

## What's in Each File

### quick-ref.md (~60 lines)
- Core composition rules (10-13, 18)
- Structure rules (8, 9, 15, 16)
- AI anti-pattern checklist
- Quick fixes table
- Task routing guide

### elements-of-style.md (~1000 lines)
- Elementary rules of usage (Rules 1-7)
- Principles of composition (Rules 8-18)
- Words and expressions commonly misused
- Full examples and explanations

### ai-writing-patterns.md (~200 lines)
- Red-flag phrases to delete
- Structural tells to fix
- Voice problems to correct
- Technical artifacts (dead giveaways)
- Quick audit process

## Key Principles

1. **Concrete > abstract** — Facts beat significance claims
2. **Active voice** — Passive weakens
3. **Omit needless words** — Every word earns its place
4. **One paragraph, one topic** — Clear structure
5. **Always check AI patterns** — Even on human drafts

## Context Efficiency

The skill is designed to minimize token usage:

- `quick-ref.md` handles 80% of cases in ~60 lines
- Full guides load only when needed
- Task routing prevents loading unnecessary content

## License

- Skill implementation: MIT License
- Elements of Style: Public domain
- Wikipedia content: CC BY-SA

## Credits

- William Strunk Jr. and E.B. White — Elements of Style
- Wikipedia contributors — AI writing detection patterns
- Distillation and optimization — Matt Jefferson
