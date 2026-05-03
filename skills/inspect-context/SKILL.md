---
description: Analyze the current context window and report a breakdown of what's consuming tokens — system prompts, files, conversation history, skills, and MCP configs. Use when the user wants to understand their token budget.
disable-model-invocation: true
---

# Context Window Inspector

Analyze everything currently loaded in this conversation's context and produce a structured report:

## What to analyze

1. **System prompt & instructions** — estimate the size of any system-level instructions, CLAUDE.md content, or project instructions currently loaded
2. **Loaded files** — list every file currently in context with approximate line counts and token estimates
3. **Conversation history** — how many turns, approximate tokens used by prior messages
4. **Skills & plugins** — list any active skills or plugin instructions loaded
5. **MCP server configs** — any MCP tool definitions adding to context
6. **Embedded documents** — any pasted documents, code blocks, or large text blocks from the user

## Output format

Present a table sorted by estimated token usage (largest first):

| Source | Type | Est. Tokens | % of Context | Needed? |
|--------|------|-------------|--------------|---------|

Then provide:
- **Total estimated context usage** as a fraction of the context window
- **Top 3 bloat candidates** — items that are large but may not be needed for the current task
- **Actionable recommendations** to reduce context usage

Be specific about file names, skill names, and content types. Use approximate token counts (1 token ≈ 4 characters for English text, less for code).