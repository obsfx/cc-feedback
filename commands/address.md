---
description: "Scan for @feedback annotations and implement the requested changes"
argument-hint: "[path or glob]"
allowed-tools: ["Read", "Edit", "Write", "Grep", "Glob", "Task"]
---

# Address Feedback Annotations

Find `@feedback` annotations and implement each requested change.

## Process

1. Grep for `@feedback` in source files. Exclude: node_modules, .git, dist, build, __pycache__, .trekker, .claude. If a path argument is given, scope to that path.

2. For each match, extract: file path, line number, feedback text, comment style used. Skip any `@feedback` that already has `@agent-response` directly below it.

3. Group by file. For each file, read it fully, then process feedbacks top-to-bottom:
   - Understand the requested change from feedback text + surrounding code
   - Implement the change using Edit
   - Add `@agent-response: <summary>` directly after the `@feedback` line, same comment style
   - Re-read the file after each edit to keep line positions accurate
   - If a feedback cannot be addressed: `@agent-response: skipped - <reason>`

4. For large codebases, use the Task tool to dispatch parallel agents per file.

5. Report summary: total found, newly addressed, skipped (already addressed or unable), brief per-change description.

## Rules

- Never remove `@feedback` annotations
- Match comment style of `@feedback` for `@agent-response`
- Keep `@agent-response` concise — max 300 characters
- Process top-to-bottom within each file
