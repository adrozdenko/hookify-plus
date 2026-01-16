# Hookify Plus

Enhanced version of the hookify plugin with additional features and bug fixes.

## Why Hookify Plus?

The upstream hookify plugin has several limitations and bugs. This fork includes fixes that are pending review in the official repo:

| Feature | Upstream | Hookify Plus |
|---------|----------|--------------|
| `not_regex_match` operator | Missing | Included |
| `value` key in conditions | Missing | Included |
| `read` event type | Missing | Included |
| Read tools trigger file rules | Bug | Fixed |

## Installation

### Option 1: Local Plugin Directory

```bash
# Clone or copy hookify-plus to your preferred location
git clone https://github.com/adrozdenko/hookify-plus ~/plugins/hookify-plus

# Use with Claude Code
claude --plugin-dir ~/plugins/hookify-plus
```

### Option 2: Replace Installed Hookify

```bash
# Backup original
mv ~/.claude/plugins/cache/claude-code-plugins/hookify/0.1.0 \
   ~/.claude/plugins/cache/claude-code-plugins/hookify/0.1.0.bak

# Link hookify-plus
ln -s /path/to/hookify-plus \
   ~/.claude/plugins/cache/claude-code-plugins/hookify/0.1.0
```

## Features

### 1. `not_regex_match` Operator

Exclude files matching a pattern:

```yaml
conditions:
  - field: file_path
    operator: regex_match
    pattern: \.tsx$
  - field: file_path
    operator: not_regex_match  # NEW!
    pattern: (\.test\.|\.stories\.)
```

### 2. `value` Key Support

Use `value` instead of `pattern` for non-regex operators (clearer intent):

```yaml
conditions:
  - field: command
    operator: contains
    value: --force  # More intuitive than "pattern"
```

### 3. `read` Event Type

Read operations (Read, Glob, Grep, LS) now have their own event type:

```yaml
event: read  # Only triggers on file reads, not edits
```

This prevents file-editing rules from incorrectly firing when reading files.

## Event Types

- `bash` - Bash commands
- `file` - File edits (Edit, Write, MultiEdit)
- `read` - File reads (Read, Glob, Grep, LS)
- `stop` - Completion checks
- `prompt` - User input
- `all` - All events

## Operators

- `regex_match` - Pattern must match (regex)
- `not_regex_match` - Pattern must NOT match (regex)
- `contains` - String must contain value
- `not_contains` - String must NOT contain value
- `equals` - Exact string match
- `starts_with` - String starts with value
- `ends_with` - String ends with value

## Upstream PRs

These features have been submitted to upstream:
- [#18419](https://github.com/anthropics/claude-code/pull/18419) - `not_regex_match` + `value` key
- [#18438](https://github.com/anthropics/claude-code/pull/18438) - `read` event type

## License

MIT
