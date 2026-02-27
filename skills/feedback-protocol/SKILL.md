---
name: feedback-protocol
description: >
  Protocol for @feedback annotations in source code. Use when user mentions
  @feedback, address feedbacks, inline feedback, or when encountering
  @feedback markers during any task.
---

# Feedback Protocol

`@feedback` annotations are inline source code comments that describe desired changes. They use the file's native comment style (`//`, `#`, `--`, `/* */`, `<!-- -->`).

## Format

```
// @feedback: <desired change>
// @agent-response: <what was done>
```

The `@agent-response` line is added directly below the `@feedback` line using the same comment style after implementing the change.

## Status

- **Unaddressed**: `@feedback` with no `@agent-response` below it
- **Addressed**: `@feedback` followed by `@agent-response` on the next line
- Only unaddressed feedbacks need action

## Workflow

1. **Discover**: Grep for `@feedback` in source files, exclude non-source dirs
2. **Parse**: Extract file path, line number, feedback text, comment style
3. **Context**: Read surrounding code to understand what the feedback refers to
4. **Implement**: Make the requested change
5. **Respond**: Add `@agent-response: <summary>` — max 300 characters. If unable: `@agent-response: skipped - <reason>`

## Proactive Behavior

When encountering `@feedback` while working on other tasks: note them to the user and offer to address if related. Do not address silently.

## Reference

For comment style details and edge cases (multi-line blocks, inline placement, nested comments): see `references/comment-patterns.md`.
