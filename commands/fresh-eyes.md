---
name: fresh-eyes
description: Look at the plan with fresh eyes
argument-hint: "[optional: path to plan document]"
---

## Step 1: Ground yourself

Reread AGENTS.md so project conventions and constraints are fresh in your mind.

## Step 2: Find the plan

If a path was provided as `$ARGUMENTS`, use that. Otherwise, find the most recent plan document (check `docs/plans/`, then the current working directory for plan-like files).

## Step 3: Fresh-eyes review

Read the ENTIRE plan document end-to-end with fresh eyes. Scrutinize every section for:

- Logical errors or contradictions between sections
- Bad assumptions (implicit or explicit)
- Missing steps or gaps in the workflow
- Conceptual mistakes or misunderstandings of the domain
- Inaccurate claims or data that doesn't hold up
- Scope creep or unnecessary complexity (violating simplicity-first)
- Conflicts with AGENTS.md conventions
- Vague language where precision is needed

## Step 4: Fix in-place

Revise the plan document directly — one small, targeted edit at a time. Do NOT rewrite the whole file in one pass. After all edits, summarize what you changed and why.
