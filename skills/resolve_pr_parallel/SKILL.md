---
name: resolve_pr_parallel
description: Resolve all PR comments using parallel processing
---

## Arguments
[optional: PR number or current PR]

Resolve all PR comments using parallel processing.

Agent automatically detects and understands your git context:

- Current branch detection
- Associated PR context
- All PR comments and review threads
- Can work with any PR by specifying the PR number, or ask it.

## Workflow

### 1. Analyze

Get all unresolved comments for PR

```bash
gh pr status
scripts/get-pr-comments PR_NUMBER
```

### 2. Plan

Create a TodoWrite list of all unresolved items grouped by type.

### 3. Implement (PARALLEL)

Spawn a pr-comment-resolver subagent for each unresolved item in parallel.

So if there are 3 comments, it will spawn 3 pr-comment-resolver subagents in parallel. like this

1. pr-comment-resolver(comment1)
2. pr-comment-resolver(comment2)
3. pr-comment-resolver(comment3)

Always run all in parallel subagents for each Task item.

### 4. Commit & Resolve

- Commit changes
- Run scripts/resolve-pr-thread THREAD_ID_1
- Push to remote

Last, check scripts/get-pr-comments PR_NUMBER again to see if all comments are resolved. They should be, if not, repeat the process from 1.
