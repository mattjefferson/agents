---
name: summarize
description: Summarize or extract text from URLs, podcasts, YouTube, and local files using the summarize CLI. Use when the user says "summarize this" or asks what a link or video is about.
---

# Summarize

Fast CLI to summarize or extract content from URLs, local files, podcasts, and YouTube links.

## When To Use

Use when:
- User says "summarize this URL/article"
- User asks "what's this link/video about?"
- User says "transcribe this YouTube/video"
- User wants to extract content from a webpage or PDF

## Prerequisites

- `summarize` CLI installed:
  - Homebrew: `brew install steipete/tap/summarize`
  - npm: `npm i -g @steipete/summarize`
- API key configured for chosen model provider (OpenRouter, Anthropic, Google, etc.)

## Commands

### Summarize URL

```bash
summarize "https://example.com/article"
```

### Summarize Local File

```bash
summarize "/path/to/file.pdf"
```

### YouTube Summary Or Transcript

```bash
# Summary
summarize "https://youtu.be/VIDEO_ID" --youtube auto

# Extract transcript/text only (no summary)
summarize "https://youtu.be/VIDEO_ID" --youtube auto --extract
```

### Model Selection

```bash
# Auto model selection (default)
summarize "https://example.com" --model auto

# Explicit model
summarize "https://example.com" --model anthropic/claude-sonnet-4-5
```

### Markdown Output (for Notes)

```bash
# Return markdown to terminal (no ANSI formatting)
summarize "https://example.com" --plain --format md --markdown-mode llm

# YouTube transcript as clean markdown
summarize "https://youtu.be/VIDEO_ID" --youtube auto --extract --plain --format md --markdown-mode llm
```

### Save Markdown Summary To Obsidian

When user asks to "summarize and save to Obsidian/vault/notes", write a `.md` note directly.

```bash
URL="https://www.youtube.com/watch?v=LG943MX-Krg"
TITLE="Strategic Patterns for Corporate AI Adoption"
OUT_DIR="$HOME/notes/00_Inbox"  # ~/notes is symlinked to the Obsidian vault root
OUT_FILE="$OUT_DIR/$(date +%F) - ${TITLE}.md"

mkdir -p "$OUT_DIR"
{
  echo "---"
  echo "created: $(date +%F)"
  echo "tags:"
  echo "  - summary"
  echo "  - needs-processing"
  echo "source: $URL"
  echo "---"
  echo
  echo "# $TITLE"
  echo
  summarize "$URL" --youtube auto --plain --format md --markdown-mode llm --length medium
} > "$OUT_FILE"

echo "Saved: $OUT_FILE"
```

If `~/notes` is not available, use:
`$HOME/Library/Mobile Documents/iCloud~md~obsidian/Documents/Notes/00_Inbox`

## Tips

- For large transcripts, return a concise summary first, then ask which section to expand (unless I ask you to summarize to notes or vault, then give the full markdown immediately).
- Use `--extract` when the user explicitly wants raw text.
- Supports common web content, PDFs, media, YouTube, and podcast links.
- For "summarize + save" requests, always return the final note path after writing the file.
