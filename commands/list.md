---
description: List all @feedback annotations in the codebase with their status and agent responses
argument-hint: "[optional: file path or glob pattern to scope the search]"
allowed-tools: ["Read", "Grep", "Glob"]
---

# List Feedback Annotations

Find and display all `@feedback` annotations in the codebase in a readable format.

## Process

### Step 1: Find all @feedback annotations

Use Grep to search for `@feedback` across the codebase.

Exclude non-source directories: node_modules, .git, dist, build, __pycache__, .trekker, .claude.

If the user provided a file path or glob pattern as an argument, scope the search to that path only.

### Step 2: Parse each annotation

For each match, read the surrounding lines to extract:
- File path and line number
- Feedback text
- Whether it has an `@agent-response` directly below it
- The agent response text if present

### Step 3: Display results

Present the results grouped by file, formatted as a table or list:

```
## src/components/App.tsx

| # | Line | Feedback | Status |
|---|------|----------|--------|
| 1 | 12 | extract this into a reusable hook | Addressed: extracted to hooks/useData.ts |
| 2 | 45 | add error boundary here | Pending |

## src/utils/api.ts

| # | Line | Feedback | Status |
|---|------|----------|--------|
| 3 | 8 | add retry logic | Addressed: added 3 retries with exponential backoff |
```

### Step 4: Show summary

At the end, show totals:

```
Summary: X total, Y addressed, Z pending
```

## Rules

- This is a read-only command, do not modify any files
- Show the full feedback text, not truncated
- For addressed feedbacks, show the agent response text after "Addressed: "
- For unaddressed feedbacks, show "Pending"
- Group results by file path
- Sort by line number within each file