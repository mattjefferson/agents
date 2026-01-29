---
name: docs-release
description: Audit and generate comprehensive user documentation for any repository
---

## Arguments
[optional: --audit to only analyze current state without changes]
[optional: --section <name> to update a specific section only]

# Release Documentation Skill

Generate and maintain comprehensive user documentation for any repository. This skill audits existing docs, identifies gaps, and creates/updates documentation following industry best practices.

## Overview

Good documentation serves different audiences:
- **End users**: How to install, configure, and use the software
- **Developers**: How to integrate, extend, or build upon it
- **Contributors**: How to set up dev environment and contribute

## Step 1: Audit Current Documentation

### 1a. Inventory Existing Docs

Check for common documentation files and patterns:

```
README.md           # Entry point - required
CONTRIBUTING.md     # Contributor guide
CHANGELOG.md        # Version history
LICENSE             # License file
docs/               # Extended documentation
  getting-started.md
  configuration.md
  api-reference.md
  guides/
  examples/
```

### 1b. Analyze README.md Quality

A complete README should include:

| Section | Purpose | Priority |
|---------|---------|----------|
| Title + badges | Identity + quick status | Required |
| Description | What it does, who it's for | Required |
| Installation | How to get it | Required |
| Quick Start | Minimal working example | Required |
| Usage | Core features and examples | Required |
| Configuration | Options and settings | If applicable |
| API Reference | For libraries/CLIs | If applicable |
| Contributing | How to help | Recommended |
| License | Legal terms | Required |

### 1c. Identify Gaps

Report what's missing or incomplete:
- Missing required sections
- Outdated installation instructions
- Missing code examples
- Broken links
- Inconsistent formatting

## Step 2: Understand the Project

Before writing docs, gather context:

### 2a. Project Type Detection

Identify the project type to tailor documentation:
- **CLI tool**: Focus on commands, flags, examples
- **Library/SDK**: Focus on API, code examples, integration
- **Web app**: Focus on deployment, configuration, usage
- **Framework**: Focus on concepts, guides, migration
- **API service**: Focus on endpoints, auth, rate limits

### 2b. Extract Key Information

Read source files to understand:
- Entry points and main exports
- Configuration options (config files, env vars)
- CLI commands and flags (if applicable)
- Public API surface
- Dependencies and requirements

### 2c. Find Existing Examples

Look for examples in:
- `examples/` directory
- Test files (often contain usage patterns)
- Existing docs or comments
- CI/CD configs (show real usage)

## Step 3: Generate/Update Documentation

### 3a. README.md Structure

Use this template as a baseline:

```markdown
# Project Name

Brief one-liner description.

## Features

- Key feature 1
- Key feature 2
- Key feature 3

## Installation

\`\`\`bash
# Package manager install
npm install project-name
\`\`\`

## Quick Start

\`\`\`javascript
// Minimal working example
import { thing } from 'project-name'
const result = thing.doSomething()
\`\`\`

## Usage

### Basic Usage
[Core functionality with examples]

### Advanced Usage
[Power user features]

## Configuration

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `option1` | string | `"default"` | What it does |

## API Reference

[For libraries: document public API]

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for development setup and guidelines.

## License

[License type] - see [LICENSE](LICENSE) for details.
```

### 3b. Extended Documentation (`docs/`)

For larger projects, create a docs directory:

```
docs/
├── index.md              # Overview and navigation
├── getting-started.md    # Installation + first steps
├── configuration.md      # All config options
├── guides/
│   ├── basic-usage.md
│   └── advanced-topics.md
├── api/
│   └── reference.md
└── examples/
    └── common-patterns.md
```

### 3c. CLI Documentation

For CLI tools, document:
- All commands and subcommands
- All flags with descriptions
- Environment variables
- Exit codes
- Real-world examples

```markdown
## Commands

### `command-name <arg> [options]`

Description of what it does.

**Arguments:**
- `arg` - What this argument is for

**Options:**
- `-f, --flag` - What this flag does
- `--option <value>` - Option with value (default: `"default"`)

**Examples:**
\`\`\`bash
# Basic usage
command-name input.txt

# With options
command-name input.txt --flag --option value
\`\`\`
```

### 3d. API/Library Documentation

For libraries, document:
- Installation for different package managers
- Import/require patterns
- All public functions/classes
- Type definitions (if typed)
- Error handling patterns

Use JSDoc/TSDoc style in code, then generate reference docs.

## Step 4: Documentation Best Practices

### Content Guidelines

1. **Start with "why"** - Explain the problem before the solution
2. **Show, don't tell** - Code examples over prose
3. **Be scannable** - Use headers, lists, tables
4. **Stay current** - Docs should match latest release
5. **Test examples** - Ensure code samples actually work

### Writing Style

- Use active voice and present tense
- Address the reader as "you"
- Keep sentences short and direct
- Define jargon on first use
- Use consistent terminology

### Code Examples

- Make examples copy-pasteable
- Include all necessary imports
- Show expected output where helpful
- Cover common use cases first
- Include error handling in advanced examples

## Step 5: Validate Documentation

### 5a. Link Checking

Verify all links work:
- Internal doc links
- External references
- Image/asset links

### 5b. Code Sample Testing

Ensure examples are correct:
- Syntax is valid
- Imports exist
- API matches current version

### 5c. Completeness Check

Verify coverage:
- All public APIs documented
- All CLI commands documented
- All config options documented
- All error codes/messages explained

## Step 6: Report Changes

Provide a summary:

```
## Documentation Update Summary

### Current State
- README: [Complete/Incomplete/Missing]
- API docs: [Complete/Incomplete/Missing]
- Examples: [X examples found]
- Guides: [X guides found]

### Changes Made
- [List of files created/updated]

### Gaps Remaining
- [Any documentation still needed]

### Recommendations
- [Suggested improvements]
```

## Audit Mode (`--audit`)

When `--audit` is specified:
- Perform full analysis
- Report current state and gaps
- Do NOT write any files
- Provide actionable recommendations

## Section Mode (`--section <name>`)

Update only a specific section:
- `readme` - README.md only
- `api` - API reference docs
- `cli` - CLI command docs
- `config` - Configuration docs
- `guides` - Usage guides

## Error Handling

- If project type unclear, ask user for context
- If existing docs conflict with code, flag for review
- Preserve custom sections when updating
- Never remove user-added content without confirmation

## Post-Documentation

After generating docs:
1. Suggest reviewing generated content for accuracy
2. Recommend adding docs to CI (link checking, etc.)
3. Remind to commit: `docs: Add/update project documentation`
