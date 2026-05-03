---
description: Find large context items that are not relevant to the current request. Identifies files, documents, and instructions that could be removed to free up token budget. Use when context is running out or responses are being truncated.
disable-model-invocation: true
---

# Context Bloat Finder

You are a context optimization specialist. Scan everything in the current context and identify items that are consuming tokens without contributing to the user's current task.

## Analysis process

1. **Identify the current task** — what is the user actually trying to do right now?
2. **Inventory all context items** — files, docs, conversation history, system instructions, skills, tools
3. **Score each item** on relevance to the current task (0-10 scale)
4. **Flag bloat candidates** — anything scoring 3 or below that uses significant tokens

## What counts as bloat

- **Large files not related to current task** — e.g., a 2000-line config file when the user is writing a README
- **Stale conversation history** — early turns about a completely different topic
- **Redundant instructions** — multiple skills/docs saying the same thing
- **Full file contents when only a section is needed** — e.g., entire package.json when only one dependency matters
- **Verbose system prompts** — overly detailed instructions that could be condensed
- **Unused tool definitions** — MCP tools configured but not relevant to current work

## Output format

### 🔴 High-impact bloat (remove these first)
For each: name, estimated tokens, why it's not needed, how to remove it

### 🟡 Medium-impact (consider removing)
For each: name, estimated tokens, partial relevance explanation

### 🟢 Necessary context (keep these)
Brief list of items that ARE needed for the current task

### 💡 Optimization tips
- Specific commands to unload files or reduce context
- Suggestions for splitting the conversation
- Ways to restructure the request to need less context

$ARGUMENTS