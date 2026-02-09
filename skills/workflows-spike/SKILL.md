---
name: workflows-spike
description: Investigate unknowns through focused research or throwaway prototypes before committing to a plan. Optional step between brainstorm and plan.
argument-hint: "[area of uncertainty or unknown to investigate]"
---

## Arguments
[area of uncertainty, unknown mechanism, or question to investigate]

# Spike: Investigate Unknowns

**Note: The current year is 2026.** Use this when dating spike documents.

A spike is a focused investigation that resolves unknowns before you commit to a plan. It answers "what would we need to do?" — not "should we do it?" The output is understanding and concrete steps, not decisions.

Some unknowns can be resolved through research — reading code, checking docs, tracing flows. Others need to be **built and experienced** — interaction feel, UX flow, behavioral validation. Spiking supports both.

**Core principles:**
- **Not about effort** — effort is implicit in the concrete steps themselves. Never estimate; just describe what's needed.
- **Investigate before proposing** — discover what already exists. You may find the system already satisfies requirements.
- **Validate, don't implement** — the spike answers a question. It's not a head start on implementation. No tests, no error handling, no edge cases.
- **Minimum to validate** — build or research the least amount needed to answer the question. Resist the urge to flesh things out.
- **Throwaway code, durable docs** — in-codebase spike code goes in a separate worktree and is deleted when the spike ends. Spike docs persist.

**When to use this skill:**
- Uncertainty about how an existing system works
- Need to discover concrete implementation steps before planning
- A brainstorm flagged unknowns that need resolution
- Exploring feasibility of an unfamiliar technology or integration
- Requirements seem right on paper but need real-world validation
- You want to look before you leap

**When NOT to use this skill:**
- Requirements are already clear — go straight to `/workflows-plan`
- You need to explore WHAT to build — use `/workflows-brainstorm` first
- The task is simple enough to just do — skip the ceremony

## Topic

<spike_topic> #$ARGUMENTS </spike_topic>

**If the topic above is empty, ask the user:** "What unknown or uncertainty would you like to investigate? Describe the area you need to understand better."

Do not proceed until you have a clear topic.

## Execution Flow

### Phase 0: Detect Resume

1. Scan `docs/spikes/*/spike.md` for documents with `Status: In Progress`.
2. For each, check if the spike doc records a branch. If so, check if that branch has a worktree (`git worktree list`).
3. If active spikes found, present them:
   - Active spikes with worktrees: name, validation goal, worktree path
   - Static HTML spikes (no worktree expected): name, validation goal, prototypes path
   - Spike docs that reference a worktree but none exists: flag the inconsistency
4. Ask the user: resume an existing spike, or start new?
5. If resuming: read the spike doc's Progress section. If the spike has a worktree, switch to it. Continue from Phase 3.
6. If starting fresh or no active spikes: proceed to Phase 1.

### Phase 1: Scope the Spike

#### 1.1 Identify What to Validate

1. **Identify what to validate.** If invoked from `/workflows-brainstorm`, the open questions provide context. If standalone, ask the user what they want to validate.
2. **Map to plan.** If a plan exists in `docs/plans/`, connect the spike to specific requirements or decisions being validated.
3. **Define the validation goal.** What are we trying to learn? What outcome would change direction? Frame it as understanding to gain, not a decision to make (see `references/spike-template.md` for acceptance guidelines).

#### 1.2 Form Questions

Formulate specific, mechanics-focused questions. Good spike questions ask about HOW things work:

- "Where is the [X] logic in the codebase?"
- "What changes are needed to achieve [Y]?"
- "How does the system currently handle [Z]?"
- "Are there constraints that affect [approach]?"
- "What does the API surface look like for [integration]?"
- "Does this interaction feel natural to use?"

**Avoid:**
- Effort estimates ("How long will this take?")
- Vague questions ("Is this hard?")
- Yes/no questions that don't reveal mechanics
- Decision questions ("Should we do X?") — decisions come after the spike

#### 1.3 Determine Spike Medium

Assess whether this spike needs research, building, or both.

**Research-only** — the answer exists in code, docs, or online. Just find it.
- Use when questions are about existing mechanics, constraints, or prior art
- No throwaway code needed

**Build to validate** — the unknown needs to be experienced. First assess: can we spike within the existing system?

- **If no** (greenfield, or relevant parts aren't built yet): default to **static HTML** and proceed.
- **If yes**: ask the user which medium fits:

| Medium | When it fits |
|---|---|
| **In-codebase** | Validating behavior that depends on real data, system integration, or existing UI. Uses a worktree. Code is throwaway. |
| **Static HTML** | Validating visual design, UX flow, interaction feel, or comparing multiple variants side-by-side. Self-contained HTML files in the spike directory. |
| **Both** | Some questions need real system context, others need rapid visual exploration. |

Default recommendation: if the validation goal is primarily visual/UX and doesn't need live data, suggest static HTML.

#### 1.4 Document and Confirm

1. **Create the spike doc on the original branch.** Use the template in `references/spike-template.md`. Save to `docs/spikes/YYYY-MM-DD-<topic>/spike.md` (ensure directory exists). Fill in Context, Validation Goal, and Approach. Status: In Progress.
2. **Present the scope to the user.** What we're validating, how we'll build it, what we're deliberately leaving out. Get confirmation before setup.

Present the questions to the user with **AskUserQuestion tool**: "Here's the spike scope and questions I'll investigate. Should I add, remove, or adjust anything?"

### Phase 2: Setup

1. **If the spike includes in-codebase work:** Invoke the `git-worktree` skill with branch name `spike/<topic>`. Record worktree info in the spike doc's Spike Code section. A single worktree can hold multiple approach variants — use separate files, components, or routes so variants coexist and are independently runnable.
2. **If the spike includes static HTML prototypes:** Create `docs/spikes/YYYY-MM-DD-<topic>/prototypes/` on the original branch.
3. **Commit durable artifacts as they're written.** Spike doc, HTML prototypes, and upstream doc updates live on the original branch — commit them incrementally. This protects against accidental loss.
4. **Minimal setup only.** No test infrastructure, no CI config. Just enough to build and demonstrate.

### Phase 3: Investigate and Validate (repeat)

#### 3.1 Research (when applicable)

Run research in parallel to answer the questions.

**Local Research (Always):**
- Task research-repo("Investigate: <spike_topic> — looking for existing patterns, mechanics, and constraints")
- Grep/Glob for relevant code, configuration, and documentation

**External Research (Conditional):**

Research externally when dealing with unfamiliar technology, third-party integrations, API surfaces, or best practices you haven't used before. Skip when questions are purely about the existing codebase.

If external research is needed:
- Task research-best-practices (spike_topic)
- Task research-framework-docs (spike_topic) — if a specific framework/library is involved
- Use Ref MCP for library/API documentation

#### 3.2 Build (when applicable)

1. **Build the minimum** to validate the goal. Deliberately rough — no tests, no error handling. If it validates the question, it's enough.
   - **For static HTML prototypes:** Each file should be self-contained (inline styles and scripts). Name files descriptively: `v1-sidebar-nav.html`, `v2-top-nav.html`. Create multiple variants when comparing approaches.
   - **For in-codebase variants:** Build each approach as separate files, components, or routes within the same worktree so the user can compare side-by-side.
2. **Present to the user.** How to experience the spike depends on what was built — open a file in a browser, run a server, execute a command, walk through the behavior. Be specific.
3. **Gather feedback.** This is a dialogue, not a single round.
   - Start with 1-2 targeted questions tied to the validation goal. Not open-ended "what do you think?" — specific questions. Examples:
     - "Does this drag interaction feel natural, or does it fight you?"
     - "Is this the information hierarchy you expected on this screen?"
     - "When you [performed action], was the result what you anticipated?"
   - Let the user respond freely — they may raise points you didn't ask about.
   - Follow up on their responses. Dig into what's working and what isn't.
   - Continue until the user's reaction is clear. Don't rush to the "what's next" options.
4. **Update the spike doc's Progress section** with a brief note: what was built/researched, key feedback, what's next.
5. **User chooses:**
   - **Iterate** — refine the current approach based on feedback, return to step 1
   - **Try a different approach** — keep the current variant, build an alternative alongside it
   - **Conclude** — enough was learned, proceed to Phase 4
   - **Abandon** — the spike isn't helping, proceed to Phase 4 with inconclusive findings
   - **Pause** — ensure Progress section is current. If a worktree exists, keep it. Resume detection (Phase 0) will find this spike in a future session.

### Phase 4: Wrap Up

1. **Finalize the spike doc:**
   - Write Findings: what was learned, what was confirmed, what surprised, what didn't work
   - Write Decisions: what changed, what was validated, requirement confirmations or changes
   - Write Concrete Steps: if we proceed, here's what we'd need to do (detailed enough for `/workflows-plan`)
   - If the spike was abandoned/inconclusive: document what was tried and why it didn't resolve the uncertainty. Carry the question forward.
   - Remove the Progress section (fold relevant content into Findings)
   - Set Status to `Complete` or `Abandoned`

2. **Propose upstream doc updates.** If a plan exists in `docs/plans/`, propose specific changes using the mapping in `references/plan-update-guide.md`. If no plan exists (standalone spike), skip to step 5.

3. **User approves** changes before they're applied. Present proposed changes clearly — what will change and why.

4. **Apply approved changes** to the plan on the original branch. Each change in the plan should include enough context that someone reading it understands the decision without reading the spike doc.

5. **Write Impact on Upstream Docs section** in spike doc: summarize what was changed in the plan, or note "Standalone spike — no upstream docs."

6. **Clean up throwaway artifacts.**
   - **In-codebase spikes:** Remove the spike worktree and delete the spike branch. All durable artifacts (spike doc, plan updates) are already on the original branch. Use the `git-worktree` skill or `git worktree remove` directly.
   - **Static HTML prototypes:** Preserved in `docs/spikes/` alongside the spike doc. No cleanup needed.

### Phase 5: Handoff

Use **AskUserQuestion tool** to present next steps:

1. If **multiple spikes were scoped**: "Spike complete. Plan updated. Ready to spike [next item], or done with spiking?"
2. If invoked from **`/workflows-brainstorm`**: return to brainstorming flow with updated context.
3. If invoked **standalone**: present options:
   - **Proceed to planning** — Run `/workflows-plan` (will auto-detect this spike)
   - **Spike something else** — Investigate another unknown
   - **Done for now** — Return later

**If user selects "Proceed to planning":**
Call `/workflows-plan` with the spike topic. The plan skill discovers spike documents during its research phase.

## Output Summary

When complete, display:

```
Spike complete!

Document: docs/spikes/YYYY-MM-DD-<topic>/spike.md

Key findings:
- [Finding 1]
- [Finding 2]

Concrete steps identified: [N]

Next: Run `/workflows-plan` when ready to turn findings into a plan.
```

## Important Guidelines

- **Investigate, don't decide** — Spike gathers information; decisions happen afterward
- **Mechanics, not effort** — Focus on HOW things work, not how long they'll take
- **Acceptance is understanding** — Complete when questions are answered and you can describe what we'd need to do
  - Good: "...we can describe how users set their language and where non-English titles appear"
  - Good: "...we can describe the steps to implement [component]"
  - Bad: "...we can answer whether this is a blocker" (that's a decision, not information)
  - Bad: "...we can decide if we should proceed" (decision comes after the spike)
- **Investigate existing systems first** — You may find the system already satisfies requirements
- **User drives iteration** — Build, present, get feedback, iterate. The user decides when they've seen enough.
- **Stay focused** — Answer the questions, don't scope-creep into planning or building features
- **Keep it throwaway** — Any code written during a spike is exploratory, not production

## Edge Cases

For handling unusual situations during spikes, consult `references/edge-cases.md`:
- When the spike invalidates the plan direction
- Coordinating multiple spikes from brainstorming
- When things go wrong (unclear goals, non-converging spikes, worktree failures)
- Cleanup issues (uncommitted worktree changes, missing worktrees)

## Anti-Patterns

Consult `references/anti-patterns.md` for common mistakes. Key ones: don't build production-quality code in a spike, don't spike on the main branch, don't put HTML prototypes in the worktree, and don't leave spike worktrees around after the spike ends.

## Reference Files

For templates, detailed guidelines, and edge cases, consult:
- **`references/spike-template.md`** — Spike document template with section descriptions and acceptance guidelines
- **`references/plan-update-guide.md`** — Mapping of spike findings to plan updates
- **`references/anti-patterns.md`** — Common anti-patterns to avoid during spikes
- **`references/edge-cases.md`** — Direction invalidation, multiple spikes, and error handling

NEVER BUILD THE FEATURE! Just investigate, validate, and document what you learn.
