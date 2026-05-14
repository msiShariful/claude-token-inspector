<div align="center">

# 🔍 Claude Token Inspector

**See exactly what's eating your context window — and reclaim it.**

[![GitHub Stars](https://img.shields.io/github/stars/msishariful/claude-token-inspector?style=flat-square&logo=github&color=yellow)](https://github.com/msishariful/claude-token-inspector/stargazers)
[![Visitors](https://visitor-badge.laobi.icu/badge?page_id=msiShariful.claude-token-inspector&style=flat-square&left_color=grey&right_color=blue&left_text=visitors)](https://github.com/msishariful/claude-token-inspector)
[![License: MIT](https://img.shields.io/badge/license-MIT-green?style=flat-square)](./LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-plugin-blueviolet?style=flat-square&logo=anthropic)](https://claude.ai/code)
[![Requires Claude Code](https://img.shields.io/badge/requires-Claude%20Code%20v1.0.33%2B-orange?style=flat-square)](https://claude.ai/code)

A Claude Code plugin that gives you a complete breakdown of your context window — what's in it, how many tokens each item costs, and exactly which items to remove for the biggest savings.

</div>

---

## ✨ Features

| Skill | Description |
|---|---|
| `/token-inspector:inspect-context` | Full token breakdown across files, conversation history, skills, MCP tools, and instructions — with percentages and estimated counts |
| `/token-inspector:find-bloat` | Identifies irrelevant context items ranked by impact, with specific commands to remove them |
| `/token-inspector:context-health` | Instant one-liner pulse check — fill level, bloat count, and one recommended action |
| `/token-inspector:slim-instructions` | Analyzes CLAUDE.md and instruction files for verbosity and redundancy, then produces a condensed rewrite |
| `/token-inspector:token-forecast` | Estimates token cost of a file, directory, or pasted content before you load it |
| `/token-inspector:mcp-audit` | Shows all connected MCP servers, their token cost, and which are unused so you can disconnect them |
| `/token-inspector:suggest-split` | Detects topic drift and recommends whether to start a new conversation, with a ready-to-paste carry-forward summary |
| `/token-inspector:history-trim` | Identifies the exact turn to `/clear` from to prune stale history without losing relevant context |

- 📊 **Instant visibility** — know your context budget at a glance
- 🎯 **Relevance scoring** — each item tagged as necessary, partial, or removable
- 💡 **Actionable output** — get exact commands, not vague advice
- ✂️ **Instruction slimming** — cut CLAUDE.md bloat without losing any behavior
- 🔌 **MCP auditing** — see which connected servers are costing tokens but going unused
- 🔮 **Token forecasting** — know the cost before you load
- ⚡ **Zero config** — works out of the box after install

---

## 📦 Installation

### Via Claude Code Marketplace (recommended)

**Step 0** — Update Claude Code to the latest version

```bash
npm update -g @anthropic-ai/claude-code
```

Verify your version afterward:

```bash
claude --version
```

You need v1.0.33 or later to use this plugin.

---

**Step 1** — Start a Claude Code session in your terminal

```bash
claude
```

Run this from your project directory. All plugin commands below are entered inside the Claude Code session.

---

**Step 2** — Add the marketplace

```
/plugin marketplace add msishariful/claude-token-inspector
```

**Step 3** — Install the plugin

```
/plugin install token-inspector@shariful-claude-plugins
```

**Step 4** — Activate

```
/reload-plugins
```

That's it. Both skills are live in your session.

---

## 🚀 Usage

### Inspect your full context

```
/token-inspector:inspect-context
```

Returns a prioritized table of everything in your context window:

| Source | Type | Est. Tokens | % of Context | Needed? |
|--------|------|-------------|--------------|---------|
| `src/api/routes.ts` | File | ~4,200 | 2.1% | Yes |
| Conversation history (turns 1–12) | History | ~8,500 | 4.3% | Partial |
| `CLAUDE.md` | Instructions | ~1,800 | 0.9% | Yes |
| `node_modules/...` read via tool | File | ~12,000 | 6.0% | No |
| ... | ... | ... | ... | ... |

Plus a summary: total usage, top bloat candidates, and recommended next steps.

---

### Find unnecessary bloat

```
/token-inspector:find-bloat
```

Categorizes every context item by relevance to your current task:

- 🔴 **High-impact bloat** — remove these first for immediate savings
- 🟡 **Medium-impact** — partially relevant, consider trimming
- 🟢 **Necessary** — keep these
- 💡 **Optimization tips** — specific commands and strategies

---

### Quick health check

```
/token-inspector:context-health
```

A two-line pulse check — no tables, no interruption:

```
🟠 71% full (~142,000 / 200,000 tokens) — Context is getting crowded — 3 bloat item(s) detected
💡 Run /token-inspector:find-bloat to identify what to remove.
```

---

### Slim your CLAUDE.md

```
/token-inspector:slim-instructions
```

Analyzes all loaded instruction files (CLAUDE.md, project settings, skill definitions) and produces a condensed rewrite with zero behavior loss:

```
### `CLAUDE.md` — 2,400 tokens → 980 tokens (59% reduction)

What was cut:
- 3 rules restated Claude's defaults (removed)
- "You should make sure to always..." pattern condensed throughout
- Duplicate examples collapsed to one per pattern

Condensed version:
[ready-to-paste rewrite]

💾 Total savings: ~1,420 tokens recovered across 1 file.
```

---

### When to use it

- Run `/context-health` at the start of any long task for a quick status
- Responses are getting truncated or feel degraded
- You've loaded many files and aren't sure what's still needed
- You want to understand your remaining context budget before starting a complex task
- Claude seems to be "forgetting" earlier instructions
- Your CLAUDE.md has grown large over time and you want to trim it
- You want to know the token cost of a file before loading it (`/token-forecast`)
- You have many MCP servers connected and aren't sure which ones you're actually using (`/mcp-audit`)
- Your session has covered many different topics and feels unfocused (`/suggest-split`)
- History has grown long but you want to keep going in the same session (`/history-trim`)
- You're working in a long session and want to clean up before continuing

---

## 🔄 Updating

```
/plugin marketplace update shariful-claude-plugins
/plugin update token-inspector@shariful-claude-plugins
/reload-plugins
```

Or enable auto-updates: run `/plugin` → **Marketplaces** tab → select `shariful-claude-plugins` → enable auto-update.

---

## 🗑️ Uninstalling

```
/plugin uninstall token-inspector@shariful-claude-plugins
```

To also remove the marketplace source:

```
/plugin marketplace remove shariful-claude-plugins
```

---

## ⚙️ Requirements

- [Claude Code](https://claude.ai/code) v1.0.33 or later

Run `claude --version` to check your current version.

---

## 📄 License

MIT © [msishariful](https://github.com/msishariful)

---

<div align="center">

If this plugin saves you time, consider giving it a ⭐ on GitHub — it helps others find it.

</div>
