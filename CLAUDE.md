# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**feedbacks** (v0.7.0) — A Claude Code plugin that finds `@feedback` annotations in source code and addresses them by implementing requested changes. Published to a custom marketplace at `obsfx/cc-feedback`.

This is a **markdown-only plugin** — no build system, no dependencies, no compiled code. All functionality is defined through markdown files that Claude Code reads to extend its capabilities.

## Architecture

The plugin follows the Claude Code plugin structure with three slash commands, one autonomous agent, and one skill:

- **Commands** (`commands/`): Each `.md` file defines a slash command via YAML frontmatter (metadata, allowed tools) followed by the natural-language protocol Claude follows when invoked.
  - `address.md` — `/feedbacks:address [path]`: Grep for `@feedback`, implement changes, add `@agent-response`
  - `list.md` — `/feedbacks:list [path]`: Read-only scan, display status table
  - `resolve.md` — `/feedbacks:resolve [path]`: Remove all annotation comment lines, leave clean code

- **Agent** (`agents/feedback-addresser.md`): Autonomous sub-agent that does the grep-parse-implement-respond cycle. Uses `model: inherit`. Also provides proactive detection — notices `@feedback` during unrelated tasks.

- **Skill** (`skills/feedback-protocol/SKILL.md`): Auto-loaded protocol knowledge triggered by phrases like "@feedback annotations". References `comment-patterns.md` for language-specific comment syntax.

- **Plugin manifest** (`.claude-plugin/plugin.json`): Name, version, description.
- **Marketplace registry** (`.claude-plugin/marketplace.json`): Custom marketplace entry for `obsfx/cc-feedback`.

## Core Protocol

The annotation state machine:

```
Unaddressed:  @feedback line present, no @agent-response below
Addressed:    @feedback followed immediately by @agent-response
Resolved:     both lines removed by /resolve
```

Annotation format adapts to the file's comment style (`//`, `#`, `--`, `/* */`, `<!-- -->`).

## Versioning

Version is tracked manually in two places that must stay in sync:
- `.claude-plugin/plugin.json` → `version`
- `.claude-plugin/marketplace.json` → `plugins[0].version`

## Key Conventions

- Excluded directories in grep scans: `node_modules`, `.git`, `dist`, `build`, `__pycache__`, `.trekker`, `.claude`
- `@agent-response` text: max 300 characters
- The `/address` command dispatches parallel agents via the `Task` tool for large codebases
- The `/list` command outputs flat markdown list: `- path:line → feedback text (status)`
- The `/resolve` command only removes annotation comment lines — never touches actual code
- Comment pattern reference at `skills/feedback-protocol/references/comment-patterns.md` covers edge cases (nested comments, multi-line blocks, inline placement)
