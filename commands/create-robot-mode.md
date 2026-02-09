---
name: create-robot-mode
description: Create agent-friendly CLI wrappers for a project's UI
argument-hint: "[project or feature to wrap]"
---

Build a "robot mode" CLI that gives coding agents the same capabilities a human gets from the UI, but optimized for agent consumption.

## Step 1: Understand the target

If `$ARGUMENTS` names a project or feature, focus on that. Otherwise, analyze the current project's UI surface — pages, forms, dashboards, actions — to determine what needs CLI equivalents.

## Step 2: Design the CLI interface

For each UI capability, create a CLI command or subcommand that:

- **Returns structured output** — JSON for data queries, markdown for narrative/reports. Pick whichever is more token-efficient and parseable for the context.
- **Mirrors the UI's information density** — an agent should get everything a human would see, nothing less.
- **Uses intuitive verb-noun naming** — `status`, `list`, `create`, `update`, `delete`, `show`. No jargon an agent wouldn't immediately understand.

## Step 3: Build the quick-start

When the CLI is invoked with **no arguments**, it must print a concise quick-start guide that:

- Lists every available subcommand with a one-line description
- Shows the 3 most common usage patterns as copy-pasteable examples
- Fits in ~30 lines — dense enough to be useful, short enough to not waste tokens

## Step 4: Implement

Build the CLI. Design choices:

- **Output format flags** — `--json` and `--md` where both formats make sense, with a sensible default
- **Minimal required args** — smart defaults over mandatory flags
- **Piping-friendly** — stdout is clean data, stderr is for human-readable messages
- **Exit codes** — 0 success, 1 error, 2 partial (so agents can branch without parsing)

## Step 5: Verify

Run the CLI with no args (quick-start), then exercise each subcommand. Confirm the output is what you'd actually want to consume as an agent — if it's not, fix it.
