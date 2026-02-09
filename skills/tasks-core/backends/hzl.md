# Backend: HZL

HZL is an external task ledger optimized for multi-agent workflows. Use the `hzl` CLI to create, claim, update, and complete tasks.

## Key Concepts

- **IDs**: Task IDs are string IDs (ULID-like), not numeric filenames.
- **Statuses**: `backlog`, `ready`, `in_progress`, `blocked`, `done`.
- **Priority**: `0`-`3` where higher is more important (`3` highest).
- **Projects**: Every task belongs to a project (`inbox` default).
- **Dependencies**: Task-level dependencies inside a project (`--depends-on`, `task add-dep`).
- **Storage**: Managed by HZL DB/config (`hzl --json which-db`).

## JSON-First Rule

Use global `--json` whenever possible:

```bash
hzl --json <command> ...
```

## Type Mapping

HZL has no first-class `type` field. Map type/category into tags:

- Bug -> include `bug` in `--tags`
- Feature -> include `feature` in `--tags`
- Docs -> include `docs` in `--tags`
- Question -> include `question` in `--tags`
- Epic/Task/Other -> include corresponding tag(s) as needed

## Field Mapping (Normalized → HZL)

| Normalized Field | HZL Field/Command | Notes |
|------------------|-------------------|-------|
| `title` | `task add "<title>"` | Required |
| `description` | `task add -d`, `task update --desc` | Markdown allowed |
| `priority` | `task add -p`, `task update -p` | `0`-`3` |
| `status` | `task add -s`, `task set-status` | Prefer `ready` at creation unless actively claimed |
| `tags` | `task add -t`, `task update -t` | Comma-separated |
| `dependencies` | `task add --depends-on`, `task add-dep` | Same-project dependencies |
| `claim` | `task claim` | Sets `in_progress` |
| `close` | `task complete` | Sets `done` |
| `work log` | `task checkpoint` / `task comment` | Use checkpoints for milestones |
| `notes/resources` | `task comment` and/or links in `description` | Keep critical context on task |

## Common Workflows

### Detecting HZL Readiness

```bash
hzl --json which-db
```

If this fails, backend is not ready.

### Ensuring Project Exists

For project-scoped workflows, ensure project exists first:

```bash
hzl --json project list
hzl --json project create <project>   # if missing
```

### Creating a New Task

```bash
hzl --json task add "Short summary" --project <project> \
  --description "Problem statement - what's broken and why it matters" \
  --priority 2 \
  --status ready \
  --tags "backend,api"
```

Optional follow-up enrichment:

```bash
hzl --json task update <id> --desc "Updated design/details"
hzl --json task comment <id> "Context, findings, references"
hzl --json task add-dep <task_id> <depends_on_id>
```

### Listing Tasks

```bash
hzl --json task list --project <project>
hzl --json task list --project <project> --status ready
hzl --json task list --project <project> --available
```

### Showing Task Details

```bash
hzl --json task show <id>
```

### Claiming Task Ownership

```bash
hzl --json task claim <id> --assignee <name>
```

Optional lease/agent tracking:

```bash
hzl --json task claim <id> --assignee <name> --agent-id <session-id> --lease 30
```

### Updating Task Fields

```bash
hzl --json task update <id> --title "New title"
hzl --json task update <id> --desc "Updated description"
hzl --json task update <id> --priority 1
hzl --json task update <id> --tags "bug,urgent"
```

### Recording Progress

```bash
hzl --json task checkpoint <id> "Implemented core flow; next: edge-case handling"
hzl --json task comment <id> "Extra context for follow-up"
```

### Completing a Task

```bash
hzl --json task complete <id>
```

### Searching

```bash
hzl --json task search "payment failure" --project <project>
```

### Validating Dependency Graph

```bash
hzl --json validate
```

## Status and Priority Guidance

- Create triaged work as `ready`.
- Use `claim` to move task to `in_progress`.
- Use `complete` to close (`done`).
- Reserve `blocked` when external dependencies prevent progress.

Priority mapping (tasks-core intent → HZL):

- Critical (`P1`) -> `3`
- Important (`P2`) -> `2`
- Nice-to-have (`P3`) -> `1`
- Backlog (`P4`) -> `0`
