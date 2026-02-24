---
description: Remove all @feedback and @agent-response annotation comments from the codebase, leaving clean code ready to commit
argument-hint: "[optional: file path or glob pattern to scope the cleanup]"
allowed-tools: ["Read", "Edit", "Grep", "Glob"]
---

# Resolve Feedback Annotations

Remove all `@feedback` and `@agent-response` annotation comment lines from the codebase, leaving clean code ready to commit.

## Process

### Step 1: Find all feedback annotations

Use Grep to search for both `@feedback` and `@agent-response` across the codebase.

Exclude non-source directories: node_modules, .git, dist, build, __pycache__, .trekker, .claude.

If the user provided a file path or glob pattern as an argument, scope the search to that path only.

### Step 2: Group by file

Organize all matches by file path to process each file once.

### Step 3: Remove annotation lines

For each file with annotations:

1. Read the full file
2. Identify all comment lines that contain `@feedback` or `@agent-response`
3. Remove those entire comment lines using Edit
4. If removing a line leaves an empty comment block or trailing blank line, clean that up too
5. Do NOT remove any non-comment code, only the annotation comment lines themselves

### Step 4: Report summary

After processing all files, provide a summary:
- Total files cleaned
- Total annotation lines removed
- List of files that were modified

## Rules

- Only remove comment lines containing `@feedback` or `@agent-response`
- Never modify actual code
- Handle multi-line feedback comments (continuation lines that are part of the same feedback block)
- Preserve file formatting and indentation
- Process files one at a time, re-reading after each edit to maintain accurate positions
- If a feedback spans multiple comment lines, remove all lines in that block