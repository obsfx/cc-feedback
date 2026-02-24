---
name: address-feedbacks
description: This skill should be used when the user mentions "@feedback annotations", "address feedbacks", "code feedback comments", "inline feedback", or when encountering @feedback markers in source code during any task. It provides the protocol for finding and addressing @feedback annotations in codebases.
---

# Address Feedbacks Protocol

## Purpose

Provide guidance for working with `@feedback` annotations - inline source code comments that describe desired changes. These annotations follow a specific protocol for marking, addressing, and tracking code change requests.

## The @feedback Annotation Pattern

### Format

An `@feedback` annotation is a source code comment containing the `@feedback:` marker followed by a description of the desired change. It uses whatever comment style is native to the file's language:

| Language | Format |
|----------|--------|
| JS/TS/Java/Go/C/Rust | `// @feedback: <text>` |
| Python/Ruby/Shell | `# @feedback: <text>` |
| HTML/XML | `<!-- @feedback: <text> -->` |
| CSS/SCSS | `/* @feedback: <text> */` |
| SQL/Lua | `-- @feedback: <text>` |

The annotation refers to the code immediately following it (or surrounding it, depending on placement).

### Response Format

After addressing a feedback, add an `@agent-response:` line directly below, using the same comment style:

```js
// @feedback: extract this into a reusable hook
// @agent-response: extracted to useDataProcessor hook in hooks/useDataProcessor.ts
export function Component() { ... }
```

### Status Detection

- **Unaddressed**: `@feedback` with no `@agent-response` below it
- **Addressed**: `@feedback` followed by `@agent-response` on the next comment line
- Only unaddressed feedbacks need action

## Addressing Workflow

### 1. Discovery

Search for `@feedback` in source files using Grep. Exclude non-source directories (node_modules, .git, dist, build, __pycache__).

### 2. Parsing

For each match, extract:
- File path and line number
- Feedback text (everything after `@feedback:`)
- Comment style used
- Whether it already has an `@agent-response`

### 3. Context Understanding

Read the surrounding code to understand what the feedback refers to. The feedback describes a change to the **nearby code**, not to the comment itself.

### 4. Implementation

Make the requested change. This could be:
- Refactoring (extract function, rename, restructure)
- Adding functionality (error handling, validation, types)
- Removing code (dead code, unnecessary complexity)
- Improving quality (performance, readability, accessibility)

### 5. Response

Add `@agent-response: <summary>` directly after the `@feedback` line. Keep it concise (under 100 chars). If the feedback cannot be addressed, respond with `@agent-response: skipped - <reason>`.

## Proactive Behavior

When encountering `@feedback` annotations while working on other tasks:
- Note them to the user
- Offer to address them if they seem related to the current work
- Do not address them silently without user awareness

## Additional Resources

### Reference Files

For detailed patterns and edge cases, consult:
- **`references/comment-patterns.md`** - Complete comment style reference and multi-line feedback handling