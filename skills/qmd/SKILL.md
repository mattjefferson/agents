---
name: qmd
description: Semantic search over the vault using QMD. For conceptual queries beyond literal grep.
user_invocable: false
github_url: https://github.com/tobi/qmd
---

# QMD Vault Search

QMD indexes the vault for semantic search. Complements literal grep with "find notes *about* this topic" queries.

## When to Use

- **Grep fails** — Searching for concept, not exact phrase
- **Fuzzy matching** — "notes about qmd best practices" not just `[[qmd best practices]]`
- **Exploration** — "what do I know about qmd?" across all notes

## MCP Tools Available

After `claude mcp add qmd -s user -- qmd mcp`:

| Tool | Use |
|------|-----|
| `qmd_search` | Fast BM25 keyword search |
| `qmd_vsearch` | Semantic vector similarity |
| `qmd_query` | Hybrid search with reranking (best quality) |
| `qmd_get` | Retrieve document by path or docid |
| `qmd_multi_get` | Retrieve multiple docs by glob/list |

## CLI Usage

```bash
# Keyword search (fast, works immediately)
qmd search "qmd best practices" -n 5

# Semantic search (requires embeddings)
qmd vsearch "preparing for weekly review" -n 5

# Hybrid with reranking (best quality)
qmd query "what's my relationship with Brewster Knowlton"

# Get full document
qmd get "notes/00_Inbox/Meeting with Brewster Knowlton.md"

# Search specific collection
qmd search "databricks" -c notes
```

## Maintenance

```bash
qmd update              # Re-index changed files
qmd status              # Check index health
qmd embed               # Update embeddings (slow)
```

## Setup (Already Done)

```bash
bun install -g https://github.com/tobi/qmd
qmd collection add ~/notes --name notes --exclude "Archive/**"
qmd context add qmd://notes "Matt's Obsidian vault"
qmd embed  # Generate vectors (one-time, slow)
claude mcp add qmd -s user -- qmd mcp
```

## Gotchas

- **Emoji filenames crash indexer** — Rename files like `🤑.md` to `money-emoji.md`
- **Archive excluded** — Old Eden notes not indexed (intentional)
- **Embeddings required for vsearch** — Keyword search works without them
- **Local models** — Uses node-llama-cpp with GGUF models, no API calls

## Related

- `/weekly` — Sunday maintenance checklist includes QMD reindex
