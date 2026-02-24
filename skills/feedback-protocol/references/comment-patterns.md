# Comment Patterns Reference

## Single-Line Comment Styles

### Double slash (`//`)
**Languages**: JavaScript, TypeScript, Java, C, C++, C#, Go, Rust, Swift, Kotlin, Scala, Dart

```js
// @feedback: use optional chaining here
// @agent-response: replaced with obj?.prop?.value
const value = obj && obj.prop && obj.prop.value;
```

### Hash (`#`)
**Languages**: Python, Ruby, Shell/Bash, Perl, R, YAML, TOML

```python
# @feedback: add type hints to this function
# @agent-response: added type hints for all parameters and return type
def process(items, threshold):
```

### Double dash (`--`)
**Languages**: SQL, Lua, Haskell, Ada

```sql
-- @feedback: add an index hint for this query
-- @agent-response: added USE INDEX hint for users_email_idx
SELECT * FROM users WHERE email = ?;
```

## Block Comment Styles

### Slash-star (`/* */`)
**Languages**: CSS, SCSS, Less, C, C++, Java, JavaScript (multiline)

```css
/* @feedback: use CSS variables for these colors */
/* @agent-response: replaced hardcoded colors with custom properties */
.header { color: #333; }
```

### HTML comment (`<!-- -->`)
**Languages**: HTML, XML, SVG, Vue templates

```html
<!-- @feedback: make this section responsive -->
<!-- @agent-response: added flex-wrap and media query for mobile -->
<div class="grid">
```

### Triple quotes (`"""` or `'''`)
**Languages**: Python (docstrings)

```python
"""
@feedback: this docstring is outdated
@agent-response: updated docstring to reflect new parameters
"""
```

## Multi-Line Feedback

When a feedback spans multiple comment lines, treat the entire block as one feedback:

```js
// @feedback: this function is doing too much -
//   split the validation logic into a separate function
//   and the transformation into another
// @agent-response: split into validateInput() and transformData()
function processData(input) {
```

```python
# @feedback: this class needs proper error handling
#   - add try/except around the API call
#   - add retry logic with exponential backoff
# @agent-response: added try/except with 3 retries and exponential backoff
class ApiClient:
```

## Inline Placement Variations

### Before the code (most common)
```js
// @feedback: simplify this condition
// @agent-response: replaced with Array.includes()
if (x === 'a' || x === 'b' || x === 'c') {
```

### After the code (end-of-line)
```js
const MAX_RETRIES = 10; // @feedback: make this configurable
// @agent-response: added to config.ts as RETRY_LIMIT with env override
```

### Inside a code block
```html
<div>
  <!-- @feedback: add aria-label for accessibility -->
  <!-- @agent-response: added aria-label and role attributes -->
  <button onclick="submit()">Go</button>
</div>
```

## Edge Cases

### Nested comments
If `@feedback` appears inside a nested comment or string literal, it should NOT be treated as a feedback annotation. Only top-level comments count.

### Multiple feedbacks in same file
Process them top-to-bottom. Be aware that addressing earlier feedbacks may shift line numbers for later ones. Re-read the file between edits if needed.

### Feedback referring to a block of code
When the feedback refers to a larger code block (function, class, section), read the entire block before making changes.

### Feedback requiring new files
If the feedback requests extracting code to a new file, create the new file and update imports/references accordingly. Note the new file path in the `@agent-response`.