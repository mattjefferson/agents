---
name: resolve_task_parallel
description: Resolve pending tasks in parallel using the unified tasks contract
---

## Arguments
[optional: specific task ID or pattern]

Resolve tasks from the configured backend in parallel.

## Workflow

### 1. Detect and Load Tasks

Run backend detection first:

```bash
/tasks detect
```

Load candidate tasks through the unified interface:

```bash
/tasks list --status pending
/tasks list --status ready
```

Apply argument filters if provided.

If any task recommends deleting, removing, or gitignoring files in `docs/plans/` or `docs/solutions/`, skip it and mark as `wont_fix`.

### 2. Plan Parallel Execution

- Create a TodoWrite list (Claude) / update_plan list (Codex) grouped by type and dependencies.
- Identify prerequisite tasks and execution order.
- Output a mermaid flow showing parallel vs sequential work.

### 3. Claim Before Work

Claim each selected task before implementation:

```bash
/tasks claim --id <id>
```

### 4. Implement in Parallel

Spawn one subagent per selected task.

Each subagent gets:
- Task ID
- Expected outcome
- Acceptance criteria
- Constraints

### 5. Resolve and Close

After work is complete:

```bash
/tasks update --id <id> --notes "Work summary, file refs, test results"
/tasks close --id <id> --reason "Completed"
```

- Commit changes.
- Push when appropriate.
