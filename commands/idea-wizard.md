---
name: idea-wizard
description: Generate and ruthlessly filter ideas for improving the project
argument-hint: "[optional: area to focus on]"
---

Come up with your very best ideas for improving this project.

## Step 1: Understand the landscape

Use subagents to explore the project in parallel — one for AGENTS.md and conventions, others for key directories and patterns. Your ideas must be grounded in what actually exists, not generic suggestions.

If `$ARGUMENTS` specifies a focus area, constrain exploration and brainstorming to that domain.

## Step 2: Brainstorm 30 ideas

Generate 30 ideas as brief one-liners. Cast a wide net — quantity first, quality later. Number them.

## Step 3: Ruthless filter

Go through each idea and critically evaluate it. Kill anything that:

- Is vague or generic ("improve error handling")
- Adds complexity without proportional value
- Duplicates something that already exists
- Conflicts with project conventions

For each rejected idea, state the reason in one line. Keep only the ideas that survive genuine scrutiny.

## Step 4: Deep dive on survivors

Spawn one subagent per surviving idea to research feasibility in parallel. Each subagent should explore the relevant code area and provide:

- **What**: Concrete, specific, actionable description with code snippets where relevant
- **Why**: What improvement it delivers and for whom
- **Downsides**: Honest risks or tradeoffs
- **Confidence**: 0–100% that it actually improves the project

Sort survivors by confidence, highest first.
