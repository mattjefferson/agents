---
name: update-agents
description: Update AGENTS.md
argument-hint: "[findings list or source type]"
---

Update the project's `AGENTS.md` with what we just learned.

## Step 1: Read current state

Read the project's `AGENTS.md` end-to-end so you know what's already there and don't duplicate or contradict existing rules.

## Step 2: Identify what to add

Look at the conversation context and `$ARGUMENTS` to extract the key findings. Only capture rules that prevent future mistakes — skip anything that's one-off or already covered.

## Step 3: Write the rules

Each new rule must be:

- **Specific and actionable** — a future agent can follow it without extra context
- **Tied to a real finding** — not hypothetical or generic advice
- **Consistent in tone and structure** — match the existing `AGENTS.md` formatting

## Step 4: Apply minimal edits

Add the new rules to the appropriate section of `AGENTS.md` using small, targeted edits. Do not reorganize or rewrite existing content. If no suitable section exists, create one that fits naturally.
