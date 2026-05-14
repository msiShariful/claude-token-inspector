<div align="center">

# 🔍 Claude Token Inspector

**See exactly what's eating your context window — and reclaim it.**

[![GitHub Stars](https://img.shields.io/github/stars/msishariful/claude-token-inspector?style=flat-square&logo=github&color=yellow)](https://github.com/msishariful/claude-token-inspector/stargazers)
[![Visitors](https://visitor-badge.laobi.icu/badge?page_id=msiShariful.claude-token-inspector&style=flat-square&left_color=grey&right_color=blue&left_text=visitors)](https://github.com/msishariful/claude-token-inspector)
[![License: MIT](https://img.shields.io/badge/license-MIT-green?style=flat-square)](./LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-plugin-blueviolet?style=flat-square&logo=anthropic)](https://claude.ai/code)
[![Requires Claude Code](https://img.shields.io/badge/requires-Claude%20Code%20v1.0.33%2B-orange?style=flat-square)](https://claude.ai/code)

A Claude Code plugin with **8 skills** for inspecting, auditing, and optimizing your context window — so you spend tokens on what matters.

</div>

---

## ✨ Skills at a glance

| Skill | What it does |
|---|---|
| [`inspect-context`](#-inspect-context) | Full token breakdown of everything in context, sorted by size, with keep/remove verdict |
| [`find-bloat`](#-find-bloat) | Finds items irrelevant to your current task and gives exact commands to remove them |
| [`context-health`](#-context-health) | Instant two-line pulse check — fill level, bloat count, one recommended action |
| [`slim-instructions`](#-slim-instructions) | Rewrites your CLAUDE.md to be shorter with zero behavior loss |
| [`token-forecast`](#-token-forecast) | Estimates token cost of a file or directory *before* you load it |
| [`mcp-audit`](#-mcp-audit) | Audits all connected MCP servers by token cost and usage |
| [`suggest-split`](#-suggest-split) | Detects topic drift and recommends whether to start a new session |
| [`history-trim`](#-history-trim) | Finds the exact `/clear` point to prune stale history safely |

---

## 📦 Installation

**Step 0** — Make sure you're on Claude Code v1.0.33 or later

```bash
npm update -g @anthropic-ai/claude-code
claude --version
```

**Step 1** — Open a Claude Code session in your terminal

```bash
claude
```

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

All 8 skills are now available in your session.

---

## 🚀 Usage

### 📊 inspect-context

**Command:** `/token-inspector:inspect-context`

**What it does:** Produces a complete, sorted breakdown of everything currently loaded in your context window — system prompts, files, conversation history, skills, MCP tool definitions, and embedded documents — with token estimates and a keep/remove recommendation for each item.

**Example output:**

```
📊 Context snapshot — ~87,400 tokens used  |  ~112,600 remaining  |  44% full

#  Source                          Type         Est. Tokens  % of Budget  Keep?
1  Conversation (turns 1–18)       History      ~32,000      16.0%        ⚠️ Partial
2  src/api/routes.ts               File         ~12,000       6.0%        ✅ Yes
3  CLAUDE.md                       Instructions  ~4,200       2.1%        ✅ Yes
4  node_modules/express/index.js   File          ~9,800       4.9%        ❌ No
5  mcp: github (22 tools)          MCP           ~6,400       3.2%        ⚠️ Maybe

🔥 Top bloat candidates
- node_modules/express/index.js — loaded early, not referenced in current task
- Turns 1–8 — initial setup discussion, no longer relevant
- mcp: github — not used since turn 3

✅ Essential context
- src/api/routes.ts, CLAUDE.md, turns 9–18

💡 Quick wins
- /clear 8 — removes first 8 turns (~14,000 tokens)
- Close node_modules/express/index.js in your editor
```

**When to use:**
- Before starting a complex task, to understand how much budget is already consumed
- When responses feel degraded or shorter than expected
- After a long session, to audit what has accumulated

---

### 🧹 find-bloat

**Command:** `/token-inspector:find-bloat`

**What it does:** Identifies the current task, scores every context item for relevance (0–10), and surfaces everything scoring 3 or below as a bloat candidate — with the exact command to remove each one.

**Example output:**

```
🧹 Found ~41,200 recoverable tokens across 4 items

🔴 Remove first — high impact
- node_modules/express/index.js (~9,800 tokens) — loaded while debugging a README, not needed. Close the file tab.
- Turns 1–7 (~18,000 tokens) — initial onboarding discussion, unrelated to current refactor. Run: /clear 7

🟡 Consider removing — medium impact
- mcp: slack (~4,800 tokens) — no Slack actions in the current task, but you may need it later.
- package-lock.json (~8,600 tokens) — only the dependency name matters, not the full lockfile.

🟢 Keep — necessary context
- src/api/auth.ts, CLAUDE.md, turns 8–now

💡 Optimization moves
- Run /clear 7 for the biggest single gain
- Consider disconnecting the slack MCP server for this session: /mcp disconnect slack
```

**When to use:**
- Responses are getting truncated or feel degraded
- You've loaded many files and aren't sure what's still needed
- You want a prioritized list of what to clean up

---

### 💚 context-health

**Command:** `/token-inspector:context-health`

**What it does:** Returns two lines — a fill-level status with colour-coded emoji and a bloat count, plus one specific recommended action. No tables, no headers, no interruption to your flow.

**Fill level indicators:**

| Level | Emoji | Status |
|-------|-------|--------|
| 0–40% | 🟢 | Context is healthy |
| 41–65% | 🟡 | Context is filling up |
| 66–80% | 🟠 | Context is getting crowded |
| 81–90% | 🔴 | Context is nearly full |
| 91–100% | 🚨 | Context is critical |

**Example output:**

```
🟠 71% full (~142,000 / 200,000 tokens) — Context is getting crowded — 3 bloat item(s) detected
💡 Run /token-inspector:find-bloat to identify what to remove.
```

```
🟢 18% full (~36,000 / 200,000 tokens) — Context is healthy — no bloat detected
💡 You're good — no action needed.
```

**When to use:**
- At the start of any task, as a quick sanity check
- Periodically during long sessions to catch fill-up early
- Anytime you want a fast answer without a full report

---

### ✂️ slim-instructions

**Command:** `/token-inspector:slim-instructions`

**What it does:** Reads all loaded instruction files (CLAUDE.md, project settings, skill definitions), identifies redundancy and verbosity, and produces a condensed rewrite of each one — preserving every behavior while cutting token cost. Shows exactly what was cut and why.

**Example output:**

```
### `CLAUDE.md` — 2,400 tokens → 980 tokens (59% reduction)

What was cut:
- 3 rules restated Claude's default behavior (removed — Claude already does these)
- "You should make sure to always..." pattern simplified to "Always..." throughout
- Two nearly-identical examples collapsed to one
- "Note that:" and "Important:" meta-prefixes removed (added no information)

Condensed version:
---
# Project guidelines

Always use TypeScript strict mode. Never commit directly to main.
Run `npm test` before every PR. Prefer composition over inheritance.
---

💾 Total savings: ~1,420 tokens recovered across 1 file. Copy the rewrite above to apply it.
```

**When to use:**
- Your CLAUDE.md has grown over time through repeated edits
- You're hitting context limits and instructions are a significant share
- You want to audit whether your instructions are actually adding value

---

### 🔮 token-forecast

**Command:** `/token-inspector:token-forecast <file-or-directory>`

**What it does:** Estimates the token cost of a file, directory, glob pattern, URL, or pasted content *before* it enters your context. Shows the budget impact and gives a safe/risky verdict so you can decide whether to load it.

**Arguments:** Provide a file path, directory, glob, or paste content directly in your message.

**Size labels:**

| Token range | Label |
|-------------|-------|
| < 1,000 | Tiny |
| 1,000–5,000 | Small |
| 5,000–20,000 | Medium |
| 20,000–60,000 | Large |
| 60,000–150,000 | Very large |
| > 150,000 | Massive — likely too big |

**Example output (single file):**

```
📦 Forecast: src/api/routes.ts — ~3,200 tokens (Small)
📊 Budget impact: 22% used + 1.6% more = 23.6% total (~153,000 tokens remaining after load)
✅ Safe to load — plenty of budget remaining.
```

**Example output (directory):**

```
📦 Forecast: src/components/ — ~38,400 tokens (Large)
📊 Budget impact: 22% used + 19% more = 41% total (~118,000 tokens remaining after load)
⚠️ Loadable but context will be getting full. Consider closing other files first.

File breakdown:
  Button.tsx         ~1,200   ✅
  Modal.tsx          ~3,400   ✅
  DataTable.tsx     ~18,600   ⚠️ Large
  DataTable.test.tsx ~15,200  ⚠️ Large

💡 Alternative: Load only Button.tsx and Modal.tsx (~4,600 tokens). Use grep to find what you need in DataTable instead of loading the whole file.
```

**When to use:**
- Before opening a large file or directory
- When planning a task that requires loading many files
- To decide between loading a file vs. using grep/search instead

---

### 🔌 mcp-audit

**Command:** `/token-inspector:mcp-audit`

**What it does:** Inventories all connected MCP servers, estimates the token cost of their tool definitions, checks which tools have actually been called this session, and flags servers that are costing tokens without contributing to your current task.

**Example output:**

```
🔌 3 MCP server(s) loaded — ~13,400 tokens in tool definitions

Server       Tools  Est. Tokens  Used This Session  Relevant?
filesystem   8      ~2,400       ✅ 3 tools used    ✅ Yes
github       15     ~6,200       ❌ None            ⚠️ Maybe
slack        12     ~4,800       ❌ None            🔴 No

🔴 Disconnect these — not used, not needed
- slack (~4,800 tokens) — no messaging actions in this task.
  Run: /mcp disconnect slack

🟡 Questionable — loaded but unused
- github (~6,200 tokens) — you haven't opened any PRs yet, but this task may involve one later.

🟢 Keep — actively used or clearly needed
- filesystem

💡 Optimization moves
- Disconnecting slack recovers ~4,800 tokens immediately
- Consider setting github to manual-connect if you only use it occasionally
```

**When to use:**
- You have many MCP servers configured and want to know what they cost
- Context is getting full and you want to reclaim token budget
- You want to audit which servers you actually use in a typical session

---

### ✂️ suggest-split

**Command:** `/token-inspector:suggest-split`

**What it does:** Maps the conversation into distinct topic blocks, scores each for carry-forward value relative to your current task, and recommends whether starting a new session would meaningfully improve context quality. If yes, produces a ready-to-paste context block for the new session.

**Example output (split recommended):**

```
✂️ Split recommended — conversation has drifted across 3 topic(s)

Topic map:
  Turns 1–8    Debugging login timeout           🗑️ Discard — resolved, no longer relevant
  Turns 9–14   Reviewing PR #42                  📋 Summarize — 2 decisions still apply
  Turns 15–now Refactoring auth middleware        ✅ Current task — carry forward

What to carry into the new conversation:
--- CONTEXT FROM PREVIOUS SESSION ---
- Decided to use JWT over sessions (PR #42 review, turn 11) — shapes the current refactor
- auth.ts middleware must stay stateless — agreed in turn 13
- Current task: refactor auth middleware in src/middleware/auth.ts
--- END CONTEXT ---

Split instructions:
1. Copy the context block above
2. Start a new Claude Code session: claude
3. Paste the context block as your first message
4. Continue with: "Continue refactoring src/middleware/auth.ts"
```

**Example output (no split needed):**

```
✅ No split needed — conversation is focused on a single task (12 turns, ~24,000 tokens used)
Context is coherent. Continue in this session.
```

**When to use:**
- A session has been running for a long time across many different tasks
- You feel like Claude has "lost the thread" of what you're working on
- Context is over 70% full and you're not sure how much is still relevant

---

### 🗂️ history-trim

**Command:** `/token-inspector:history-trim`

**What it does:** Reads the full conversation history, classifies every block of turns as an active dependency, resolved context, or dead weight, finds the latest safe trim point, and gives you the exact `/clear N` command to run — along with a warning for any active dependencies you'd lose.

**Example output:**

```
✂️ Safe to clear 13 turns — recover ~26,000 tokens

Turns  Summary                                    Status               Safe to Clear?
1–5    Initial project setup discussion           Resolved             ✅ Yes
6–10   Debugged login timeout — fixed in auth.ts  Resolved             ✅ Yes
11–13  Decided to use JWT over sessions           Active dependency    🔴 No — still shapes current work
14–now Current task: refactor auth middleware      Current              —

💡 Recommended: /clear 10

⚠️ Before clearing: note this from turn 11: "Decided to use JWT over sessions — this decision still shapes the current refactor."
Save that note before running /clear.
```

**Example output (full clear is safe):**

```
✅ Full clear is safe — /clear will lose nothing relevant to your current task.
💡 Run: /clear
```

**When to use:**
- History has grown long but you want to stay in the same session (vs. starting over)
- You know early turns are stale but aren't sure which ones are safe to remove
- You want to free up token budget without losing any active context

---

## 🗺️ Which skill should I use?

| Situation | Recommended skill |
|-----------|------------------|
| I want to know my current token usage | `context-health` or `inspect-context` |
| Responses are getting truncated | `find-bloat` |
| I want a full breakdown of everything | `inspect-context` |
| My CLAUDE.md has grown too long | `slim-instructions` |
| I want to know the cost before loading a file | `token-forecast` |
| I have many MCP servers and want to audit them | `mcp-audit` |
| My session has drifted across many topics | `suggest-split` |
| History is long but I want to stay in this session | `history-trim` |

---

## 🔄 Updating

```
/plugin marketplace update shariful-claude-plugins
/plugin update token-inspector@shariful-claude-plugins
/reload-plugins
```

Or enable auto-updates: run `/plugin` → **Marketplaces** tab → select `shariful-claude-plugins` → toggle auto-update on.

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
