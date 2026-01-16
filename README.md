# Hookify Plus

[![Version](https://img.shields.io/badge/version-0.1.0--plus.3-blue)](CHANGELOG.md)
[![Based on](https://img.shields.io/badge/based%20on-hookify%200.1.0-gray)](https://github.com/anthropics/claude-code/tree/main/plugins/hookify)

Enhanced hookify plugin for Claude Code with community fixes and features.

## Quick Start

```bash
# 1. Clone
git clone https://github.com/adrozdenko/hookify-plus ~/hookify-plus

# 2. Backup & link (recommended)
mv ~/.claude/plugins/cache/claude-code-plugins/hookify/0.1.0{,.bak}
ln -s ~/hookify-plus ~/.claude/plugins/cache/claude-code-plugins/hookify/0.1.0

# 3. Create a rule
cat > .claude/hookify.warn-rm.local.md << 'EOF'
---
name: warn-dangerous-rm
enabled: true
event: bash
pattern: rm\s+-rf
---
⚠️ **Dangerous rm command!** Double-check the path before proceeding.
EOF

# Done! The rule is active immediately.
```

## Why Hookify Plus?

The upstream hookify plugin has bugs and missing features. This fork integrates community fixes:

| Issue | Upstream | Hookify Plus |
|-------|----------|--------------|
| `not_regex_match` operator | ❌ Missing | ✅ Added |
| `value` key in conditions | ❌ Missing | ✅ Added |
| `read` event type | ❌ Missing | ✅ Added |
| Global rules (`~/.claude/`) | ❌ Missing | ✅ Added |
| `Update` tool support | ❌ Missing | ✅ Added |
| Read tools fire file rules | 🐛 Bug | ✅ Fixed |
| Write tool `new_text` field | 🐛 Bug | ✅ Fixed |
| Python 3.8 type hints | 🐛 Bug | ✅ Fixed |
| Claude can't see block reasons | 🐛 Bug | ✅ Fixed |
| Windows paths with spaces | 🐛 Bug | ✅ Fixed |
| Example uses wrong operator | 🐛 Bug | ✅ Fixed |

**11 improvements** over upstream. No waiting for Anthropic to merge PRs.

## Installation

### Recommended: Symlink (works everywhere)

```bash
# Clone to home directory
git clone https://github.com/adrozdenko/hookify-plus ~/hookify-plus

# Backup original and create symlink
mv ~/.claude/plugins/cache/claude-code-plugins/hookify/0.1.0 \
   ~/.claude/plugins/cache/claude-code-plugins/hookify/0.1.0.bak

ln -s ~/hookify-plus \
   ~/.claude/plugins/cache/claude-code-plugins/hookify/0.1.0
```

### To Revert

```bash
rm ~/.claude/plugins/cache/claude-code-plugins/hookify/0.1.0
mv ~/.claude/plugins/cache/claude-code-plugins/hookify/0.1.0.bak \
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
    operator: not_regex_match  # Exclude test/story files
    pattern: (\.test\.|\.stories\.)
```

### 2. `value` Key Support

Use `value` instead of `pattern` for non-regex operators:

```yaml
conditions:
  - field: command
    operator: contains
    value: --force  # Clearer than "pattern" for substring match
```

### 3. `read` Event Type

Read operations have their own event type (no more false triggers):

```yaml
event: read  # Only fires on Read, Glob, Grep, LS
```

### 4. Global Rules

Rules in `~/.claude/` apply to ALL projects:

```bash
~/.claude/hookify.no-console-log.local.md  # Works in every project
```

Both global and project rules are loaded and evaluated together.

### 5. Claude Sees Block Reasons

When a rule blocks, Claude now receives the full message explaining why:

```yaml
action: block  # Claude sees your message and can correct itself
```

## Event Types

| Event | Tools |
|-------|-------|
| `bash` | Bash |
| `file` | Edit, Write, MultiEdit, Update |
| `read` | Read, Glob, Grep, LS |
| `stop` | Stop (completion check) |
| `prompt` | UserPromptSubmit |
| `all` | All of the above |

## Operators

| Operator | Description |
|----------|-------------|
| `regex_match` | Pattern matches (regex) |
| `not_regex_match` | Pattern does NOT match (regex) |
| `contains` | Substring present |
| `not_contains` | Substring NOT present |
| `equals` | Exact match |
| `starts_with` | Starts with value |
| `ends_with` | Ends with value |

## Credits

Fixes integrated from community PRs and issues:

| Contributor | Contribution |
|-------------|--------------|
| [@adrozdenko](https://github.com/adrozdenko) | `not_regex_match`, `value` key, `read` event |
| [@kp222x](https://github.com/kp222x) | Global rules ([#13916](https://github.com/anthropics/claude-code/pull/13916)) |
| [@heathdutton](https://github.com/heathdutton) | Write fix + Update tool ([#16081](https://github.com/anthropics/claude-code/pull/16081)) |
| Issue reporters | [#14588](https://github.com/anthropics/claude-code/issues/14588), [#12446](https://github.com/anthropics/claude-code/issues/12446), [#16152](https://github.com/anthropics/claude-code/issues/16152), [#13464](https://github.com/anthropics/claude-code/issues/13464) |

## Updating

```bash
cd ~/hookify-plus && git pull
```

Changes take effect immediately—no restart needed.

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for version history.

## License

MIT
