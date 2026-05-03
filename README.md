<div align="center">

# 🔍 Claude Token Inspector

**See exactly what's eating your context window — and reclaim it.**

[![GitHub Stars](https://img.shields.io/github/stars/msishariful/claude-token-inspector?style=flat-square&logo=github&color=yellow)](https://github.com/msishariful/claude-token-inspector/stargazers)
[![GitHub Downloads](https://img.shields.io/github/downloads/msishariful/claude-token-inspector/total?style=flat-square&logo=github&label=downloads&color=blue)](https://github.com/msishariful/claude-token-inspector/releases)
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

- 📊 **Instant visibility** — know your context budget at a glance
- 🎯 **Relevance scoring** — each item tagged as necessary, partial, or removable
- 💡 **Actionable output** — get exact commands, not vague advice
- ⚡ **Zero config** — works out of the box after install

---

## 📦 Installation

### Via Claude Code Marketplace (recommended)

**Step 1** — Add the marketplace

```
/plugin marketplace add msishariful/claude-token-inspector
```

**Step 2** — Install the plugin

```
/plugin install token-inspector@shariful-claude-plugins
```

**Step 3** — Activate

```
/reload-plugins
```

That's it. Both skills are live in your session.

### Local development / testing

```bash
git clone https://github.com/msishariful/claude-token-inspector.git
claude --plugin-dir ./claude-token-inspector
```

After editing any `SKILL.md`, run `/reload-plugins` to pick up changes.

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

### When to use it

- Responses are getting truncated or feel degraded
- You've loaded many files and aren't sure what's still needed
- You want to understand your remaining context budget before starting a complex task
- Claude seems to be "forgetting" earlier instructions
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
