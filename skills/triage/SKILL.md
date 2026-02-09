---
name: triage
description: Triage and categorize findings for the unified task system
---

## Arguments
[findings list or source type]

- Use `/tasks detect` first to select and validate backend.
- Use `/tasks` operations for all task mutations.
- Do not embed backend-specific command branches in this workflow.

Present findings one-by-one for triage. The goal is to decide whether each item should become a tracked task.

**IMPORTANT: DO NOT CODE DURING TRIAGE.**

## Workflow

### Step 0: Detect Backend

Run:

```bash
/tasks detect
```

Capture and state selected backend.

### Step 1: Present Each Finding

Use this format:

```text
---
Issue #X: [Brief Title]

Severity: 🔴 P1 / 🟡 P2 / 🔵 P3
Category: [Security/Performance/Architecture/Bug/Feature/etc.]
Location: [file_path:line_number]

Description:
[Detailed explanation]

Problem Scenario:
[What fails and impact]

Proposed Solution:
[How to fix]

Estimated Effort: [Small|Medium|Large]
---
Do you want to add this to the task list?
1. yes - create or update task
2. next - skip this item
3. custom - modify details before creating
```

### Step 2: Handle Decision

When user says `yes`:

1. Map severity to priority: `P1/P2/P3` -> `p1/p2/p3` (File) or `P1/P2/P3` (Beads via backend mapping).
2. Create or update through `/tasks` contract:

```bash
/tasks create --title "..." --priority "..." --type "..." --description "..."
/tasks update --id <id> --notes "..." --acceptance-criteria "..."
```

3. Add triage approval/work-log note via update path:

```bash
/tasks update --id <id> --notes "Approved during triage. Ready for work."
```

4. Confirm in normalized form:
   - `✅ Approved: <id/title> - ready for implementation`

When user says `next`:

- Skip item and track it in summary.
- If the item already exists and user wants it explicitly closed, use:

```bash
/tasks close --id <id> --reason "Not pursuing"
```

When user says `custom`:

- Collect modified fields and repeat Step 2.

### Step 3: Continue Until Complete

- Process all findings.
- Track progress with TodoWrite/update_plan as needed.

### Step 4: Final Summary

Return:

```markdown
## Triage Complete

**Total Items:** [X]
**Approved:** [Y]
**Skipped:** [Z]

### Approved Tasks
- [id] - [title] - [priority]

### Skipped
- [finding] - [reason]

### Next Steps
1. `/resolve_task_parallel`
2. Work individual tasks as needed
```

## Guardrails

- If a recommendation suggests deleting or gitignoring `docs/plans/` or `docs/solutions/`, skip and mark as `wont_fix`.
- Keep all task persistence operations routed through `/tasks`.
