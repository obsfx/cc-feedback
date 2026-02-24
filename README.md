# feedbacks

A Claude Code plugin that finds `@feedback` annotations in source code and addresses them by implementing the requested changes.

## Installation

```bash
# Add the marketplace
claude plugin marketplace add obsfx/cc-feedback

# Install the plugin
claude plugin install feedbacks
```

Or in Claude Code interactive mode:

```
/plugin marketplace add obsfx/cc-feedback
/plugin install feedbacks
```

## How It Works

Sprinkle `@feedback` comments in your code to describe desired changes. The plugin finds them, implements the changes, and leaves an `@agent-response` for you to review.

**Before:**
```js
// @feedback: extract this into a reusable hook
export function Component() {
  const [data, setData] = useState([]);
  useEffect(() => { fetchData().then(setData); }, []);
  return <List items={data} />;
}
```

**After:**
```js
// @feedback: extract this into a reusable hook
// @agent-response: extracted to hooks/useData.ts
export function Component() {
  const data = useData();
  return <List items={data} />;
}
```

## Supported Comment Styles

Works with any language's comment syntax:

| Style | Languages |
|-------|-----------|
| `// @feedback: ...` | JS, TS, Java, Go, C, Rust, Swift |
| `# @feedback: ...` | Python, Ruby, Shell, YAML |
| `/* @feedback: ... */` | CSS, SCSS, C (block) |
| `<!-- @feedback: ... -->` | HTML, XML, Vue |
| `-- @feedback: ...` | SQL, Lua, Haskell |

## Usage

### Command: `/feedbacks:address`

Explicitly scan and address all `@feedback` annotations in the codebase:

```
/feedbacks:address
/feedbacks:address src/components/
```

- Finds all `@feedback` annotations
- Skips already-addressed ones (those with `@agent-response`)
- Implements each change
- Adds `@agent-response: <summary>` for review

### Command: `/feedbacks:list`

List all `@feedback` annotations with their status and agent responses:

```
/feedbacks:list
/feedbacks:list src/components/
```

### Command: `/feedbacks:resolve`

Remove all `@feedback` and `@agent-response` annotation lines from the codebase, leaving clean code ready to commit:

```
/feedbacks:resolve
/feedbacks:resolve src/components/
```

### Proactive Detection

The plugin also includes an agent that notices `@feedback` annotations while working on other tasks and offers to address them.

## Review Workflow

1. Add `@feedback: <desired change>` comments where you want changes
2. Run `/feedbacks:address`
3. Review the changes and the `@agent-response` summaries
4. Run `/feedbacks:resolve` to clean up annotations and commit

## Components

| Component | Purpose |
|-----------|---------|
| **Command** (`/address`) | Scan and address all feedbacks |
| **Command** (`/list`) | List all annotations with status |
| **Command** (`/resolve`) | Remove all annotation comments for clean commit |
| **Skill** (`feedback-protocol`) | Protocol knowledge, auto-loaded when relevant |
| **Agent** (`feedback-addresser`) | Autonomous worker, proactive detection |

## License

MIT
