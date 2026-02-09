---
name: zhrsc-performance
description: Profile and improve ~/.zshrc startup performance with timing + zprof
argument-hint: "[optional: run count, default 10]"
---

Analyze `~/.zshrc` startup performance and return concrete optimization recommendations.

## Step 1: Baseline startup time

Use `$ARGUMENTS` as run count if provided; default to `10`.

Measure startup time with multiple runs:

```bash
for i in $(seq 1 <runs>); do
  /usr/bin/time -p zsh -i -c exit >/dev/null
done
```

Report mean and slowest run.

## Step 2: Profile hotspots with zprof

Profile function-level startup cost using temporary markers in `~/.zshrc`:

1. Backup `~/.zshrc`
2. Insert at top (only if missing):
   - `zmodload zsh/zprof`
3. Insert at end (only if missing):
   - `zprof`
4. Run one interactive startup and capture output:
   - `zsh -i -c exit`
5. Restore original `~/.zshrc` from backup

Do not leave profiling markers behind.

## Step 3: Inspect common bottlenecks

Check for known slow patterns in `~/.zshrc` and sourced files:

- Repeated/uncached `compinit`
- Heavy version managers at startup (`nvm`, `pyenv`, `rbenv`, `asdf`)
- Large plugin/theme loading (Oh My Zsh / zinit / antigen / etc.)
- Expensive command substitutions and external process calls
- Duplicate sourcing of the same scripts

## Step 4: Recommendations

Provide prioritized recommendations with estimated impact:

1. **High impact** (largest measured wins first)
2. **Medium impact**
3. **Low impact / cleanup**

Each recommendation must include:

- Why it is slow (evidence from timing/zprof)
- Exact file/line reference
- Concrete change to apply
- Expected impact (qualitative or measured)

## Step 5: Output format

Return:

- Baseline timing summary (runs, mean, slowest)
- Top `zprof` hotspots (top 5 by total time)
- Prioritized recommendations
- Optional “quick wins first” 3-step action plan
