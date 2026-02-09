---
name: prove-it
description: Prove to me this works
argument-hint: "[optional: specific feature or change to verify]"
---

Prove to me this works. Do not just say it works — demonstrate it.

## Step 1: Identify what to prove

If `$ARGUMENTS` specifies a feature or change, focus on that. Otherwise, look at the most recent changes (git diff, recent commits, in-progress work) and identify what needs verification.

## Step 2: Build the evidence

Use every applicable method to prove correctness:

- **Run tests** — execute existing tests that cover the change. If none exist, write one.
- **Run the code** — execute it and show real output, not hypothetical output.
- **Diff behavior** — compare before vs after when relevant.
- **Check edge cases** — try inputs that could break it.
- **Read logs/errors** — surface any warnings or failures.

Do NOT skip a method because you "already know" it works. Run it anyway.

## Step 3: Verdict

Present the evidence clearly. State pass or fail for each check. If anything fails, fix it and re-prove — don't just report the failure.
