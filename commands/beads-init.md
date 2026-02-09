---
description: Initialize beads in the current repo
argument-hint: "[prefix, e.g. 'pho' for prompthoarder]"
---

# Initialize Beads Workspace

Set up beads_rust (`br`) issue tracking in the current repository.

## Steps

### 1. Get the Prefix

If no prefix was provided as an argument, ask the user:

> What prefix should I use for issue IDs? (e.g., `pho` for prompthoarder, `mgr` for manager — typically 2-4 lowercase letters derived from the project name)

### 2. Check for Existing Workspace

```bash
br where 2>/dev/null
```

If a `.beads/` already exists, warn the user and stop — don't overwrite without explicit consent.

### 3. Initialize

```bash
br init --prefix=<PREFIX> --json
```

### 4. Verify Setup

```bash
br info --json
br stats --json
```

Confirm the prefix, database path, and JSONL export are correct.

### 5. Add AGENTS.md Instructions

Inject beads workflow instructions into the repo's AGENTS.md so all agents know how to use beads:

```bash
br agents --add --force
```

Then append the `bv` (beads viewer) sidecar instructions to AGENTS.md. Insert after the `br` block (after `<!-- end-br-agent-instructions -->`):

```bash
{
  echo ""
  echo "<!-- bv-agent-instructions-v1 -->"
  cat ~/projects/agent-scripts/templates/BV-AGENTS.md
  echo ""
  echo "<!-- end-bv-agent-instructions -->"
} >> AGENTS.md
```

### 6. Ensure .beads/ Is Gitignored Correctly

The `.beads/.gitignore` should already exclude `*.db`, `*.db-shm`, `*.db-wal`, `*.lock`, `last-touched`, and `*.tmp`. Verify it exists:

```bash
cat .beads/.gitignore
```

If the repo's root `.gitignore` excludes `.beads/` entirely, warn — the JSONL and config should be tracked in git.

### 7. Summary

Print:
- Workspace location
- Prefix and example issue ID format (e.g., `pho-a1b`)
- What's tracked in git (`.beads/issues.jsonl`, `.beads/config.yaml`, `.beads/metadata.json`)
- What's gitignored (`.beads/*.db`, WAL files, locks)
- Reminder: run `br sync --flush-only` before committing to export DB to JSONL
