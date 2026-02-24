# address-feedbacks

A Claude Code plugin that finds `@feedback` annotations in source code and addresses them by implementing the requested changes.

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

### Command: `/address-feedbacks`

Explicitly scan and address all `@feedback` annotations in the codebase:

```
/address-feedbacks
/address-feedbacks src/components/
```

- Finds all `@feedback` annotations
- Skips already-addressed ones (those with `@agent-response`)
- Implements each change
- Adds `@agent-response: <summary>` for review

### Proactive Detection

The plugin also includes an agent that notices `@feedback` annotations while working on other tasks and offers to address them.

## Review Workflow

1. Add `@feedback: <desired change>` comments where you want changes
2. Run `/address-feedbacks`
3. Review the changes and the `@agent-response` summaries
4. Remove the comment blocks when satisfied

## Components

| Component | Purpose |
|-----------|---------|
| **Command** (`/address-feedbacks`) | Explicit scan and address all feedbacks |
| **Skill** (`address-feedbacks`) | Protocol knowledge, auto-loaded when relevant |
| **Agent** (`feedback-addresser`) | Autonomous worker, proactive detection |

## Installation

### From local directory

```bash
claude --plugin-dir /path/to/cc-address-feedbacks
```

### From marketplace

```bash
/install address-feedbacks
```
