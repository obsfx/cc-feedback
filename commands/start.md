---
description: Scan the codebase for @feedback annotations and address each one by implementing the requested changes
argument-hint: "[optional: file path or glob pattern to scope the search]"
allowed-tools: ["Read", "Edit", "Write", "Grep", "Glob", "Task"]
---

# Address Feedback Annotations

Scan the project source code for `@feedback` annotations and address each feedback by implementing the requested code changes.

## Process

### Step 1: Find all @feedback annotations

Use Grep to search for `@feedback` across the codebase:

```
Pattern: @feedback
```

Exclude common non-source directories: node_modules, .git, dist, build, __pycache__, .trekker, .claude.

If the user provided a file path or glob pattern as an argument, scope the search to that path only.

### Step 2: Parse each feedback

For each match found, extract:
- **File path** and **line number**
- **Feedback text**: everything after `@feedback:` or `@feedback` on the same line (and continuation lines if the comment block continues)
- **Comment style**: detect whether it uses `//`, `#`, `/* */`, `<!-- -->`, `--`, `"""`, etc.
- **Surrounding code context**: read the file to understand what code the feedback refers to

### Step 3: Skip already-addressed feedbacks

If a `@feedback` annotation already has a corresponding `@agent-response:` reply directly below it (in the same comment block), skip it - it has already been addressed.

### Step 4: Address each feedback

For each unaddressed feedback, in order of file appearance:

1. **Read the file** to understand the full context around the feedback
2. **Understand the request**: the feedback text describes a desired change to the code near the annotation
3. **Implement the change**: modify the code as requested using Edit tool
4. **Add response**: immediately after the `@feedback` comment line, add a new comment line using the same comment style:
   ```
   @agent-response: <brief summary of what was changed>
   ```

### Step 5: Report summary

After processing all feedbacks, provide a summary:
- Total `@feedback` annotations found
- How many were addressed (new)
- How many were skipped (already had `@agent-response`)
- For each addressed feedback: file, line, what was changed

## Comment Style Examples

The `@agent-response` must use the same comment style as the `@feedback`:

```js
// @feedback: extract this to a utility function
// @agent-response: extracted to utils/processData.ts
```

```python
# @feedback: add type hints to this function
# @agent-response: added type hints for all parameters and return type
```

```html
<!-- @feedback: make this section responsive -->
<!-- @agent-response: added flex-wrap and media query for mobile -->
```

```css
/* @feedback: use CSS variables for these colors */
/* @agent-response: replaced hardcoded colors with var(--primary) etc. */
```

## Rules

- Never remove `@feedback` annotations - they are for human review
- Always add `@agent-response` directly after the `@feedback` line, in the same comment style
- Keep `@agent-response` text concise (one line, under 100 chars)
- If a feedback is ambiguous, implement the most reasonable interpretation and note it in the response
- If a feedback cannot be addressed (e.g., requires external dependencies not present), add `@agent-response: skipped - <reason>`
- Process feedbacks file-by-file to minimize re-reading
- For large changes, use the Task tool to dispatch parallel agents per file