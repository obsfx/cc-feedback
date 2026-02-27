---
description: "List all @feedback annotations with their status"
argument-hint: "[path or glob]"
allowed-tools: ["Read", "Grep", "Glob"]
---

# List Feedback Annotations

Scan for `@feedback` annotations and display their status.

## Process

1. Grep for `@feedback` in source files. Exclude: node_modules, .git, dist, build, __pycache__, .trekker, .claude. If a path argument is given, scope to that path.

2. For each match, read surrounding lines to determine:
   - File path and line number
   - Feedback text
   - Whether `@agent-response` exists on the next line (and its text)

3. Print results as a flat markdown list:

```
- src/components/App.tsx:12 → extract this into a reusable hook (addressed: extracted to hooks/useData.ts)
- src/components/App.tsx:45 → add error boundary here (pending)
- src/utils/api.ts:8 → add retry logic (addressed: added 3 retries with exponential backoff)

Summary: 3 total, 2 addressed, 1 pending
```

Format per item: `- path:line → feedback text (status)`

Status is either `pending` or `addressed: <agent-response text>`.

## Rules

- Read-only — do not modify any files
- Show full feedback text
- Sort by file path, then line number
