---
name: gmail
description: Gmail operations with status-aware checks using gog CLI. Use when checking inbox, viewing threads, verifying sent status, or replying to email.
---

# Gmail Skill

Status-aware Gmail operations using `gog`. Prevents draft-vs-sent confusion.

## When To Use

Use when:
- Checking inbox or unread replies
- Inspecting a Gmail thread
- Verifying whether a message was sent or is still a draft
- Drafting, sending, or deleting drafts

## Commands

### Check Inbox

```bash
gog gmail search "newer_than:1d" --plain | head -20
```

### View Thread With Status Check

When viewing a thread, verify message status labels before reporting delivery status.

```bash
# Thread overview
gog gmail thread get <thread_id>

# Verify message labels for sent/draft status
gog gmail get <message_id> --plain | grep "label_ids"
```

Interpret labels:
- `label_ids` contains `SENT`: message was sent.
- `label_ids` contains `DRAFT`: message is still draft and not sent.

### Check Drafts

```bash
gog gmail drafts list --plain
```

### Detect Cora Multi-Option Drafts

Cora can create multi-option drafts separated by `###`. These require human selection before sending.

```bash
# Inspect draft details
gog gmail drafts get <draft_id>
```

If body contains `###`, treat as a multi-option draft and do not auto-send.

### Reply To Thread Safely

```bash
gog gmail send \
  --thread-id "<thread_id>" \
  --to "<recipient_email>" \
  --subject "Re: <original_subject>" \
  --body "<message_body>"
```

Always confirm with user before executing `send`.

### Delete Draft

```bash
gog gmail drafts delete <draft_id> --force
```

Always confirm with user before deleting drafts.

## Status Reporting

When reporting email state, be explicit:
- `SENT`: confirmed sent (SENT label present)
- `DRAFT`: not sent yet (DRAFT label present)
- `CORA_DRAFT`: Cora multi-option draft detected (`###` separator)

## Common Patterns

### "Did this email go out?"

```bash
gog gmail get <message_id> --plain | grep "label_ids"
```

Check for `SENT` vs `DRAFT`.

### "Check for replies"

```bash
gog gmail search "newer_than:1d is:unread" --plain
```

### "What did Cora draft?"

```bash
gog gmail drafts list --plain
# Then inspect candidate draft IDs with:
gog gmail drafts get <draft_id>
```
