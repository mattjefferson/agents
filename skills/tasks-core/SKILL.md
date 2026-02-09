---
name: tasks-core
description: Backend-agnostic task adapter. Detects and validates the configured backend (File, Beads, or HZL), then routes task operations through a single contract.
argument-hint: "<detect|create|list|show|update|claim|close|search> [arguments]"
---

# Tasks Core Skill

Use this skill as the single task backend adapter for all task workflows.

- Public command surface: `/tasks ...`
- Backend implementations: `backends/file.md`, `backends/beads.md`, and `backends/hzl.md`
- Rule: Consumers should call this skill/command contract and avoid embedding backend-specific branching logic.

## Operation Contract

Supported operations:

- `detect` — detect and validate backend, return capabilities
- `create` — create task/issue
- `list` — list tasks with filters
- `show` — show task details by ID
- `update` — update task fields
- `claim` — atomically assign and mark in-progress
- `close` — complete/close task
- `search` — full-text search

Use JSON output when the underlying command supports it.

## Normalized Output Shape

Return and summarize results with this normalized envelope where possible:

```json
{
  "backend": "file|beads|hzl",
  "operation": "detect|create|list|show|update|claim|close|search",
  "ok": true,
  "id": "optional-id",
  "status": "optional-status",
  "priority": "optional-priority",
  "title": "optional-title",
  "items": [],
  "raw": "backend-specific raw output if needed"
}
```

## Backend Detection and Validation

### Detection Order

1. Check if `.beads/` exists.
2. If `.beads/` exists, require `br` CLI.
3. Validate Beads workspace with:

```bash
br where --json
```

4. If `.beads/` does not exist, check for HZL markers:
   - `.hzl/` exists, or
   - `.config/hzl/` exists, or
   - `HZL_DB`, `HZL_DB_EVENTS_PATH`, or `HZL_CONFIG` environment variable is set, or
   - `project_tracker: hzl` is present in `AGENTS.md` or `CLAUDE.md`.

5. If an HZL marker is present, require `hzl` CLI and validate with:

```bash
hzl --json which-db
```

6. Select backend:
   - `.beads/` exists + `br` present + `br where --json` succeeds -> **Beads backend**
   - `.beads/` exists but `br` missing -> **hard stop** with install guidance
   - `.beads/` exists, `br` present, but invalid workspace -> **hard stop** with remediation
   - HZL marker exists + `hzl` present + `hzl --json which-db` succeeds -> **HZL backend**
   - HZL marker exists but `hzl` missing/invalid -> **hard stop** with install guidance
   - otherwise -> **File backend**

7. State backend explicitly:
   - `Using tasks-core (Beads backend)`
   - `Using tasks-core (HZL backend)`
   - `Using tasks-core (File backend)`

### Fail-Closed Errors

If `.beads/` exists but Beads cannot be used, do not silently fall back to file backend.

Use this remediation guidance:

- Install Beads CLI
- Ensure `.beads/` workspace is initialized and healthy
- Verify with `br where --json` and `br info --json`

If an HZL marker is present but HZL cannot be used, do not silently fall back to file backend.

Use this remediation guidance:

- Install HZL CLI (`brew tap tmchow/hzl && brew install hzl` or `npm install -g hzl-cli`)
- Verify with `hzl --json which-db`
- If needed, initialize with `hzl init`

## Operation Routing

After detection, read only the selected backend guide:

- File backend: `backends/file.md`
- Beads backend: `backends/beads.md`
- HZL backend: `backends/hzl.md`

Then execute the requested operation using that backend's command conventions.

### Expected Arguments by Operation

- `detect`: no extra args
- `create`: title, priority, type/category, description, tags/dependencies (optional)
- `list`: status/priority/label filters (optional)
- `show`: ID (required)
- `update`: ID + updated fields
- `claim`: ID
- `close`: ID + reason
- `search`: query string

If required arguments are missing, stop and ask for only the missing fields.

## Shared Concepts

### Epic Policy (Beads)

When backend is Beads, apply this policy consistently:

- Use `type=epic` for capability workstreams with clear user/business outcomes.
- Do not create epics for generic implementation phases by default.
- Prefer dependencies (`br dep add`) for sequencing work.
- If work is a single capability slice with <=5 tightly related tasks, use tasks-only (no epic).
- If an epic would contain only one task, prefer task-only unless there is a clear justification.

### Priority Mapping

| Level | File Backend | Beads Backend | HZL Backend |
|-------|-------------|---------------|-------------|
| Critical | `p1` | `P1` | `3` |
| Important | `p2` | `P2` | `2` |
| Nice-to-have | `p3` | `P3` | `1` |
| Backlog | — | `P4` | `0` |

### Work Log Format

All backends should preserve this work log structure (file section, Beads comment, or HZL checkpoint/comment):

```markdown
### YYYY-MM-DD - Session Title

**By:** Claude Code / Developer Name

**Actions:**
- Specific changes made (include file:line references)
- Commands executed
- Tests run
- Results of investigation

**Learnings:**
- What worked / what didn't
- Patterns discovered
- Key insights for future work
```

### Todo Tracking Note

TodoWrite/update_plan are in-session tracking tools only. They do not replace persistent tasks managed by this skill.

## Adding a New Backend

When adding a backend, keep this contract stable and add backend-specific implementation under `backends/<name>.md`.

Required operations: `create`, `list`, `show`, `update`, `claim`, `close`, `search`.

Detection must be fast, local, and validated before routing.
