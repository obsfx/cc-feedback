---
description: "Remove all @feedback and @agent-response annotation comments, leaving clean code"
argument-hint: "[path or glob]"
allowed-tools: ["Read", "Edit", "Grep", "Glob"]
---

# Resolve Feedback Annotations

Remove all `@feedback` and `@agent-response` comment lines from source files.

## Process

1. Grep for `@feedback` and `@agent-response` in source files. Exclude: node_modules, .git, dist, build, __pycache__, .trekker, .claude. If a path argument is given, scope to that path.

2. Group by file. For each file:
   - Read the full file
   - Remove comment lines containing `@feedback` or `@agent-response`
   - Clean up empty comment blocks or trailing blank lines left behind
   - Re-read after each edit for accurate positions

3. Report: files cleaned, annotation lines removed, list of modified files.

## Rules

- Only remove comment lines containing `@feedback` or `@agent-response`
- Never modify actual code
- Handle multi-line feedback blocks (remove all continuation lines)
- Preserve file formatting and indentation
