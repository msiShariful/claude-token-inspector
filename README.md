# Claude Token Inspector

A Claude Code plugin that helps you understand and optimize your context window usage. Find what's eating your tokens and reclaim your context budget.

## Features

| Skill | What it does |
|---|---|
| `/token-inspector:inspect-context` | Full breakdown of everything consuming your context window — files, conversation history, skills, MCP tools — with estimated token counts and percentages |
| `/token-inspector:find-bloat` | Hunts for large context items that aren't relevant to your current task and gives actionable removal steps |

## Installation

### 1. Add the marketplace

```
/plugin marketplace add msishariful/claude-token-inspector
```

### 2. Install the plugin

```
/plugin install token-inspector@shariful-claude-plugins
```

### 3. Activate

```
/reload-plugins
```

That's it. Both skills are now available in your session.

## Usage

### Inspect your full context

```
/token-inspector:inspect-context
```

Returns a table like:

| Source | Type | Est. Tokens | % of Context | Needed? |
|--------|------|-------------|--------------|---------|
| `src/api/routes.ts` | File | ~4,200 | 2.1% | Yes |
| Conversation history (turns 1-12) | History | ~8,500 | 4.3% | Partial |
| `CLAUDE.md` | Instructions | ~1,800 | 0.9% | Yes |
| ... | ... | ... | ... | ... |

Plus a summary with total usage, top bloat candidates, and recommendations.

### Find unnecessary bloat

```
/token-inspector:find-bloat
```

Categorizes every context item by relevance to your current task:

- 🔴 **High-impact bloat** — remove these first for immediate savings
- 🟡 **Medium-impact** — partially relevant, consider removing
- 🟢 **Necessary** — keep these
- 💡 **Optimization tips** — specific commands and strategies to reduce usage

### When to use

- Your responses are getting truncated or degraded
- You've loaded many files and aren't sure what's still needed
- You want to understand how much context budget remains before starting a complex task
- You're debugging why Claude seems to "forget" earlier instructions

## Updating

When a new version is released:

```
/plugin marketplace update shariful-claude-plugins
/plugin update token-inspector@shariful-claude-plugins
/reload-plugins
```

Or enable auto-updates: run `/plugin`, go to the **Marketplaces** tab, select `shariful-claude-plugins`, and enable auto-update.

## Uninstalling

```
/plugin uninstall token-inspector@shariful-claude-plugins
```

To also remove the marketplace:

```
/plugin marketplace remove shariful-claude-plugins
```

## Local development

Clone and test locally without installing:

```bash
git clone https://github.com/msishariful/claude-token-inspector.git
claude --plugin-dir ./claude-token-inspector
```

After editing any `SKILL.md`, run `/reload-plugins` inside Claude Code to pick up changes.

## Requirements

- Claude Code v1.0.33 or later (run `claude --version` to check)

## License

MIT