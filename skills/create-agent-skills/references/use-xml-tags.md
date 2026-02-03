# XML Tags in Skills (Deprecated)

## Current Guidance

**Use standard markdown headings, not XML tags.**

Per the official Claude Code skill specification:
- YAML frontmatter for metadata
- Standard markdown body with headings (##, ###)
- No XML wrapper tags

## Historical Context

Earlier versions of skill guidance recommended XML tags for structure. This has been superseded by the markdown-based format which is:
- Easier to read and write
- More familiar to developers
- Equally parseable by Claude

## Migration

If you have skills using XML tags:

```xml
<!-- Old format (deprecated) -->
<objective>Do something</objective>
<quick_start>...</quick_start>
```

Convert to:

```markdown
## Objective
Do something

## Quick Start
...
```

## When XML Is Still Appropriate

XML can still be useful within skill content for:
- Structured data examples
- Template outputs
- Configuration examples

But the skill structure itself should use markdown headings.
