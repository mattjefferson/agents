# Spike Document Template

Use this template when creating spike documents. Update the Progress section as the spike proceeds. Finalize Findings, Decisions, and Impact when the spike concludes.

## Document Structure

```markdown
# Spike: [Topic]

**Date:** [date]
**Status:** In Progress
**Branch:** spike/[topic, if applicable]
**Plan:** [path, if exists]
```

**Valid status values:** `In Progress`, `Complete`, `Abandoned`

## Sections

### Context (always include)

```markdown
## Context
[Why this spike exists. What uncertainty it addresses. Which plan requirements
or decisions need validation, if applicable.]
```

### Validation Goal (always include)

```markdown
## Validation Goal
[What we're trying to validate. What outcome would change direction.]

Spike is complete when [what understanding/confidence we'll have — describe
the information, not a decision].
```

**Acceptance guidelines:**

The validation goal describes the understanding we'll have, not a conclusion:

- "...we can describe how the drag interaction feels and whether users find it intuitive" (understanding)
- "...we know whether the reduced scope still delivers value to users" (understanding)
- "...we can describe the steps to implement [component]" (understanding)
- NOT "...we can decide if we should proceed" (that's a decision made after the spike)
- NOT "...we can answer whether this is a blocker" (decision, not information)

The spike gathers information through research and building. Decisions are made afterward based on what was learned.

### Approach (always include)

```markdown
## Approach
[How the spike was conducted — research, in-codebase prototype, static HTML,
or combination. Key scoping decisions. What was deliberately left out.]
```

### Progress (update during spike, remove when finalizing)

A working note for resuming the spike if paused. Brief — not a play-by-play log.

```markdown
## Progress
[What's been explored/built so far. Key feedback received. What's next.]
```

When the spike concludes, fold relevant content into Findings and Decisions, then remove this section.

### Findings (finalize when spike concludes)

```markdown
## Findings
[What was learned. What was confirmed. What surprised us. What didn't work.
Include file paths, line numbers, and code snippets where relevant.]
```

### Decisions (finalize when spike concludes)

```markdown
## Decisions
- [Decision]: [Rationale]
- [Requirement confirmed/changed]: [Why]
```

### Concrete Steps (finalize when spike concludes)

```markdown
## Concrete Steps
If we proceed, here's what we'd need to do:

1. [Specific step with file references]
2. [Specific step]
3. [Specific step]
```

These should be detailed enough to feed directly into `/workflows-plan`.

### Impact on Upstream Docs (finalize when spike concludes)

```markdown
## Impact on Upstream Docs
[Summary of plan changes made based on this spike.
Each change should also be documented in the plan itself with full rationale —
the plan should be self-sufficient for downstream stages without needing to
read this spike doc.

For standalone spikes: "Standalone spike — no upstream docs."]
```

### Spike Code (include when code was written)

```markdown
## Spike Code
**Worktree:** [path, if applicable]
**Branch:** spike/[topic, if applicable]
**Prototypes:** docs/spikes/YYYY-MM-DD-<topic>/prototypes/ [if static HTML prototypes exist]
**Variants:** [list of approach variants and where to find them, if multiple approaches explored]
**Reusable:** No
```

In-codebase spike code is throwaway by default. If any is genuinely reusable (rare), describe what specifically and where. Static HTML prototypes are preserved in the spike directory as reference artifacts.
