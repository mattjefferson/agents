# Beads Task Template

Use this template to structure content for Beads fields. Each section maps to a specific `br` field.

---

## title

Brief, actionable summary (1 line).

Example: "Fix N+1 query in user dashboard"

---

## type

Choose one: `task` | `bug` | `feature` | `docs` | `question` | `epic`

---

## priority

`P1` (critical) | `P2` (important) | `P3` (nice-to-have) | `P4` (backlog)

---

## description

**What's broken and why it matters.** This is the problem statement.

Example:
```
User dashboard loads slowly due to N+1 queries when displaying team members.
Page load time exceeds 3s for users with 50+ team members, causing complaints
and potential churn. This blocks the enterprise rollout.
```

---

## design

**Technical approach, solutions, and architecture.** Use this for HOW to solve.

```markdown
## Proposed Solutions

### Option 1: Eager Loading
**Approach:** Add `includes(:team_members)` to dashboard query.
**Pros:** Simple, minimal code change
**Cons:** Loads all data upfront
**Effort:** 30 minutes
**Risk:** Low

### Option 2: Pagination + Caching
**Approach:** Paginate team members, cache user data.
**Pros:** Scales better long-term
**Cons:** More complex, UI changes needed
**Effort:** 4 hours
**Risk:** Medium

## Technical Details

**Affected files:**
- `app/controllers/dashboard_controller.rb:24` - main query
- `app/views/dashboard/index.html.erb` - team member loop

**Related components:**
- TeamMember model
- DashboardSerializer

**Database changes:** None required
```

---

## notes

**Findings, resources, and context.** Investigation results and references.

```markdown
## Findings

- Root cause: `@user.team_members` called in view without eager loading
- 47 queries observed for user with 50 team members
- Similar issue fixed in #892 for projects page

## Resources

- PR: #1287
- Related issue: #456
- AppSignal trace: [link]
- Rails guide on eager loading: [link]

## Recommended Action

Implement Option 1 (eager loading) as immediate fix. Consider Option 2
for phase 2 if enterprise accounts exceed 200 team members.
```

---

## acceptance_criteria

**Definition of done.** Testable checklist items.

```markdown
- [ ] Dashboard loads in < 500ms for 100 team members
- [ ] No N+1 queries in test suite
- [ ] Existing tests pass
- [ ] Code reviewed and approved
```

---

## estimated_minutes

Effort from chosen solution. Example: `30` or `240`

---

## external_ref

Primary external reference. Example: `PR#1287` or `JIRA-456`

---

## labels

Tags for filtering. Example: `performance`, `database`, `rails`

---

## comments (work log)

Add each work session as a separate comment using `br comments add`.

```markdown
### 2026-02-03 - Initial Investigation

**By:** Claude Code

**Actions:**
- Profiled dashboard with rack-mini-profiler
- Identified 47 queries for N+1 pattern
- Reviewed similar fix in PR #892

**Learnings:**
- `includes()` must be on controller query, not model scope
- rack-mini-profiler badge shows query count in dev
```

---

## Quick Create Commands

```bash
# 1. Create with description
br create --title="Fix N+1 in dashboard" --type=bug --priority=2 \
  --description="Dashboard slow due to N+1 queries on team_members" --json

# 2. Add technical design
br update <id> --design="$(cat <<'EOF'
## Proposed Solutions
### Option 1: Eager Loading
...
## Technical Details
...
EOF
)" --json

# 3. Add findings and context
br update <id> --notes="$(cat <<'EOF'
## Findings
- Root cause: ...
## Resources
- PR: #1287
EOF
)" --json

# 4. Add acceptance criteria
br update <id> --acceptance-criteria="- [ ] Load time < 500ms
- [ ] No N+1 in tests" --json

# 5. Set estimate and labels
br update <id> --estimate=30 --add-label "performance" --json

# 6. Add work log
br comments add <id> --message="### 2026-02-03 - Started investigation..." --json
```
