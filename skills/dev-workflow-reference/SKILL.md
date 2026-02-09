---
name: dev-workflow-reference
description: Reference for compound-engineering workflows, review skills, solutions KB, and git worktrees. Consult when starting development work.
user_invocable: false
---

# Development Workflow Reference

For non-trivial coding tasks, use compound-engineering workflows.

## Workflow Selection Guide

| Workflow | When to Use | Output |
|----------|-------------|--------|
| `/workflows-brainstorm` | Unclear requirements, exploring WHAT to build | `docs/brainstorms/*.md` |
| `/workflows-plan` | Clear goal, need HOW to implement | `docs/plans/*.md` |
| `/plan-deepen` | Plan exists but needs research depth | Enhanced plan with best practices |
| `/workflows-work` | Plan approved, ready to execute | Code + tests |
| `/workflows-review` | Code complete, quality-critical | Multi-agent review findings |
| `/workflows-compound` | Just solved something tricky | Learning in `~/docs/solutions/` |
| `/lfg` | Full autonomous loop (all above) | End-to-end with video |

**Flow options:**
```
Simple task:
  plan → work → compound (if tricky)

Complex feature:
  brainstorm → plan → plan-deepen → work → review → compound

Full autonomous:
  /lfg (runs the entire chain)
```

## Review Agents

Invoke via Task tool when reviewing code:

| Agent | Use For |
|-------|---------|
| `review-code-simplicity` | YAGNI check, remove unnecessary complexity |
| `analyze-patterns` | Detect anti-patterns, ensure consistency |
| `review-security` | OWASP, secrets, input validation |
| `analyze-performance` | Bottlenecks, scalability concerns |
| `review-typescript` | High-bar TS review |
| `review-python` | High-bar Python review |

**When to invoke:**
- After implementing new skills → `review-code-simplicity`
- After modifying Claude config → `analyze-patterns`
- Before shipping anything external → `review-security`

## Compound Engineering Mindset

- 80% planning + review, 20% execution
- Plans are prompts — good plans enable one-shot implementation
- After tricky fixes, always `/workflows-compound` to capture learnings
- After significant experiences, prompt for compound extraction — what's reusable?

## Solutions Knowledge Base

`~/docs/solutions/` contains structured learnings with YAML frontmatter. The `research-learnings` skill queries this before work starts.

**Structure:**
```
docs/solutions/
├── ai-tooling/           # Tool crashes, API quirks
├── browser-automation/   # Playwright, agent-browser, Chrome
├── claude-config/        # Settings, hooks, MCP
├── skills/               # Skill design patterns
├── workflow-issues/      # Process problems
├── best-practices/       # Patterns worth documenting
└── patterns/
    └── critical-patterns.md  # ALWAYS check before work
```

**Adding learnings:**
1. After solving a tricky problem, run `/workflows-compound`
2. Or manually create a file with frontmatter (see `~/docs/solutions/schema.md`)

**How it compounds:**
- `/workflows-plan` auto-invokes `research-learnings` to check for relevant past solutions
- `/plan-deepen` pulls in applicable patterns
- Critical patterns are always checked regardless of task type

**When to add:** Problem required non-obvious debugging, same issue could recur, solution isn't obvious from docs, pattern applies beyond this specific case.

## Git Workflow

**Use git worktrees** for non-trivial work. Matt runs multiple agents sessions simultaneously — worktrees prevent file conflicts.

**Setup:**
```
~/projects/<repo>/                ← main checkout
~/projects/<repo>/.worktrees/
  ├── feat-topic-a/               ← session 1
  └── fix-bug-b/                  ← session 2
```

**Commands:**
- Create: `git worktree add .worktrees/feat-topic -b feat/topic main && cd .worktrees/feat-topic`
- List: `git worktree list`
- Remove: `git worktree remove .worktrees/feat-topic`

**Fallback:** If worktree not set up, create a feature branch (`feat/<topic>` or `fix/<topic>`).

Trivial changes (typos, one-line fixes) can go directly to main.
