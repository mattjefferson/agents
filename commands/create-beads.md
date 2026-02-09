---
description: Create beads from a plan
argument-hint: "[path to plan file]"
---

# Create Beads from Plan

Read the plan file and transform it into a structured set of beads using `br` (beads_rust CLI).

## Process

### 1. Analyze the Plan

Read the plan file thoroughly. Identify:
- **Epics**: Major capability workstreams that deliver a user/business outcome (type=epic)
- **Tasks**: Concrete work items within each epic
- **Dependencies**: What blocks what — ordering between tasks
- **Priority**: Critical path items get P1-P2, nice-to-haves get P3-P4

### 1.5 Decide Epic vs Task-Only Mode

Use this decision rule before creating issues:

- **Create epics** when:
  - The plan has 2+ capability slices/workstreams, or
  - Any single slice has 3+ tasks, or
  - Cross-slice dependency management is required.
- **Skip epics (tasks-only)** when:
  - The plan is a single slice with <=5 tightly related tasks.

Important:
- Do **not** use generic implementation phases (e.g., `Phase 1`, `Phase 2`) as epics by default.
- A phase should be an epic only if it is independently shippable and outcome-focused.

### 2. Ensure Workspace Exists

```bash
br where --json 2>/dev/null || br init --json
```

### 3. Create Epics First

Create top-level epics for each major capability workstream.

Each epic should include:
- **Objective**: what outcome this workstream delivers
- **Scope boundary**: what is in/out
- **Done definition**: what makes the epic complete

```bash
br create --title="Epic: [Workstream Name]" --type=epic --priority=<P> \
  --description="[Objective + scope boundary + done definition]" --json
```

### 4. Create Tasks Under Epics

For each task, populate ALL relevant fields — make beads self-contained so any agent can pick one up cold:

```bash
# Create task with parent epic (required when epics are used)
br create --title="[Actionable summary]" --type=<task|bug|feature|docs> \
  --priority=<P1-P4> --parent=<epic_id> \
  --description="[Problem statement — what and why]" \
  --labels="<comma-separated>" \
  --estimate=<minutes> --json
```

Parenting rule:
- If epics are being used, every non-standalone task should have `--parent=<epic_id>`.
- If you chose tasks-only mode in Step 1.5, omit `--parent` and keep tasks flat.

Then enrich with structured fields:

```bash
# Technical design — HOW to solve it
br update <id> --design="$(cat <<'EOF'
## Approach
[Concrete implementation steps]

## Affected Files
- path/to/file.ext — [what changes]

## Considerations
- [Constraints, edge cases, risks]
EOF
)" --json

# Acceptance criteria — definition of done
br update <id> --acceptance-criteria="$(cat <<'EOF'
- [ ] [Testable criterion 1]
- [ ] [Testable criterion 2]
- [ ] Tests pass
EOF
)" --json

# Notes — context our future self needs
br update <id> --notes="$(cat <<'EOF'
## Context
- [Why this approach was chosen]
- [What alternatives were considered]
- [Key decisions and reasoning]

## References
- [Links, related work, prior art]
EOF
)" --json
```

### 5. Wire Up Dependencies

After all tasks exist, add dependency edges:

```bash
# Task B depends on Task A (A blocks B)
br dep add <task_B_id> <task_A_id> --json
```

### 6. Validate

```bash
# Check for cycles
br dep cycles --json

# Verify ready queue makes sense (should show leaf tasks with no blockers)
br ready --json

# Overview
br stats --json
br epic status --json
```

Validation checklist:
- No dependency cycles.
- Every task is parented when epics are used (or intentionally standalone if tasks-only mode).
- No accidental orphan tasks when epics exist.
- Epics should generally have multiple children; single-task epics should be justified.
- `br ready --json` returns a sensible set of actionable starting tasks.

## Quality Standards

Every bead must be **self-contained and self-documenting**:

- **description**: Clear problem statement — what's broken/missing and why it matters
- **design**: Concrete technical approach — files, patterns, architecture decisions
- **acceptance_criteria**: Testable checklist — how we know it's done
- **notes**: Context and reasoning — why this approach, what we considered, references
- **estimate**: Realistic time in minutes from the plan
- **labels**: Categorical tags for filtering (e.g., `backend`, `api`, `database`, `ui`)
- **priority**: Reflects critical path position (P1=critical, P2=important, P3=nice-to-have, P4=backlog)

A new contributor should be able to read any single bead and understand:
1. What needs to be done
2. Why it matters
3. How to approach it
4. When it's done
5. What depends on it

## Output

After creating all beads, print a summary:
- Total epics and tasks created
- Tasks per epic
- Unparented task count (should be 0 when epics are used)
- Dependency graph overview (`br graph --json` if available, otherwise `br dep tree <root_epic> --json`)
- Dependency cycle status
- Ready queue (`br ready --json`)
- Any warnings (missing estimates, orphan tasks, etc.)
