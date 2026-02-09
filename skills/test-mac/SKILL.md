---
name: test-mac
description: Run native macOS UI smoke tests using the Peekaboo CLI. Use when validating desktop app flows and regressions.
---

## Arguments
[app name, bundle id, or 'frontmost']

# Mac UI Test Command

<command_purpose>Use Peekaboo CLI to run macOS native app UI smoke tests, capture artifacts, and route failures through `/tasks`.</command_purpose>

## CRITICAL: Use Peekaboo CLI Only

Use `peekaboo` commands for interaction and capture.
Do not use browser automation tools or iOS simulator tools for native macOS app testing.

## Introduction

<role>QA Engineer specializing in native macOS UI validation</role>

This command tests desktop app flows and catches regressions that unit tests may miss:
- Broken primary workflows
- UI render/layout problems
- Command/menu interaction failures
- Permission and integration breakages

## Prerequisites

<requirements>
- Peekaboo CLI installed and available in PATH
- Screen Recording and Accessibility permissions granted
- Target macOS app installed locally
</requirements>

## Setup

**Check installation:**
```bash
command -v peekaboo >/dev/null 2>&1 && echo "Installed" || echo "NOT INSTALLED"
peekaboo --version
```

**Check required permissions:**
```bash
peekaboo permissions
```

## Main Tasks

### 0. Verify Peekaboo Setup

<check_peekaboo_installed>

Before starting ANY macOS testing, verify Peekaboo is available and permissions are granted:

```bash
command -v peekaboo >/dev/null 2>&1 && echo "Ready" || echo "NOT INSTALLED"
peekaboo --version
peekaboo permissions
```

If Peekaboo is missing or permissions are not granted, inform the user and stop.

</check_peekaboo_installed>

### 1. Ask Execution Mode

<ask_execution_mode>

Before running tests, ask user whether to run stepwise or faster batch mode:

Use AskUserQuestion with:
- Question: "How should I run macOS UI tests?"
- Options:
  1. **Interactive (watch)** - Run stepwise with screenshots at each major action
  2. **Batch (faster)** - Run the same flow with fewer pauses

Store the choice and adapt cadence:
- Interactive: pause after each major step and present artifacts
- Batch: execute full flow then summarize

</ask_execution_mode>

### 2. Resolve Target App and Window

<test_target> $ARGUMENTS </test_target>

<resolve_target>

Determine the app under test from `$ARGUMENTS`:
- `frontmost`: use the currently focused app/window
- app name or bundle id: target that application explicitly

Helpful discovery commands:
```bash
peekaboo list apps --json
peekaboo list windows --json
```

If target cannot be resolved confidently, stop and ask the user to select app/window.

</resolve_target>

### 3. Capture Baseline State

<baseline_capture>

Capture an annotated UI map before interactions:

```bash
peekaboo see --annotate --path /tmp/test-mac-baseline.png
peekaboo image --mode frontmost --retina --path /tmp/test-mac-baseline-ui.png
```

If testing a specific app/window, include app/window targeting flags.

</baseline_capture>

### 4. Run Smoke Test Flow

<test_flows>

Run these core flows where applicable:
1. Launch/focus app
2. Primary navigation
3. Key user action (create/edit/save/open)
4. Preferences/settings
5. Close and reopen critical view

For each critical flow (launch, primary screen, key action, preferences/settings, close/reopen):

1. Snapshot UI and identify targets:
```bash
peekaboo see --annotate --path /tmp/test-mac-step.png
```
2. Interact with controls:
```bash
peekaboo click --on B1
peekaboo type "example input"
peekaboo press return
peekaboo hotkey --keys "cmd,s"
```
3. Capture evidence:
```bash
peekaboo image --mode frontmost --retina --path /tmp/test-mac-after-step.png
```
4. Record pass/fail and notes for summary table.

</test_flows>

### 5. Human Verification (When Required)

<human_verification>

Pause for human input when testing touches:

| Flow Type | What to Ask |
|-----------|-------------|
| External sign-in flow | "Please complete sign-in and confirm success" |
| macOS permissions dialog | "Please grant the permission and verify expected behavior" |
| Keychain / secure storage | "Please confirm keychain prompt and successful save/load" |
| Notification delivery | "Please confirm a test notification appears" |
| File picker / sandbox access | "Please choose a test file/folder and confirm open/save works" |

Use AskUserQuestion:
```markdown
**Human Verification Needed**

This test requires [flow type]. Please:
1. [Action to take]
2. [What to verify]

Did it work correctly?
1. Yes - continue testing
2. No - describe the issue
```

</human_verification>

### 6. Handle Failures

<failure_handling>

When a check fails:

1. **Document the failure:**
   - Capture screenshot and annotated UI map
   - Capture exact reproduction steps
   - Note app/window context

2. **Ask user how to proceed:**
   ```markdown
   **Test Failed: [flow/step]**

   Issue: [description]
   Context: [app/window]

   How to proceed?
   1. Fix now - I'll help debug and fix
   2. Create task - Use `/tasks` (tasks-core routing)
   3. Skip - Continue testing other flows
   ```

3. **If "Fix now":**
   - Investigate likely root cause
   - Apply fix
   - Re-run the failing flow

4. **If "Create task":**
   - Route through `/tasks`:
```bash
/tasks detect
/tasks create --title "macOS UI test failure: <short title>" --type bug --priority P1 --description "<impact and repro>"
/tasks update --id <id> --notes "Peekaboo evidence + repro steps" --acceptance-criteria "- [ ] Failure no longer reproduces in test-mac validation"
```
   - Continue testing

5. **If "Skip":**
   - Mark as skipped in notes
   - Continue testing

</failure_handling>

### 7. Test Summary

<test_summary>

After all tests complete, present summary:

```markdown
## Mac UI Test Results

**Target App:** [name/bundle id]
**Target Window:** [window title/id]
**Mode:** [Interactive / Batch]

### Flows Tested: [count]

| Flow | Status | Notes |
|------|--------|-------|
| Launch/Focus | ✅ Pass | |
| Primary Navigation | ✅ Pass | |
| Key Action | ❌ Fail | [issue summary] |
| Settings | ⏭️ Skip | Requires manual credential |

### Artifacts Captured: [count]
- /tmp/test-mac-baseline.png
- /tmp/test-mac-after-step.png

### Human Verifications: [count]
- [flow]: [confirmed/failed]

### Failures: [count]
- [flow] - [issue description]

### Created Tasks: [count]
- `{task-id}` - macOS UI test failure task (created via `/tasks`)

### Result: [PASS / FAIL / PARTIAL]
```

</test_summary>

### 8. Cleanup

<cleanup>

After testing:
- Optionally close/relaunch app to confirm clean exit behavior
- Keep artifact paths in summary for easy follow-up

Optional commands:
```bash
peekaboo app quit --app "<AppName>"
peekaboo clean
```

</cleanup>

## Quick Usage Examples

```bash
# Test frontmost native app
/test-mac frontmost

# Test a specific app
/test-mac "Safari"

# Test by bundle id
/test-mac "com.example.MyMacApp"
```
