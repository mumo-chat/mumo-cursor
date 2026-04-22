# mumo — Cursor plugin

Multi-model deliberation as MCP tools, with an auto-triggering skill. Consult a panel of models (Claude, GPT, Gemini, Grok, Qwen, GLM, Kimi) on contested decisions — architecture choices, plan reviews, pricing tradeoffs, strategy — where a single model might be confidently wrong.

Works in Cursor 2.5+. For Claude Code and Cowork, see [`mumo-chat/mumo-mcp`](https://github.com/mumo-chat/mumo-mcp).

## What's in the box

- **MCP server** — `https://mumo.chat/api/mcp`, five tools: `create_deliberation`, `append_round`, `get_session`, `list_sessions`, `list_models`
- **Auto-triggering skill** — `skills/mumo/SKILL.md` tells the agent *when* to reach for the panel (architecture tradeoffs, plan reviews, contested decisions)

## Install

```bash
# In Cursor, open the Command Palette and run:
Cursor: Add Plugin from GitHub

# Then paste:
https://github.com/mumo-chat/mumo-cursor
```

Or install via marketplace once listed at [cursor.com/marketplace](https://cursor.com/marketplace).

You'll also need a mumo API key:

1. Sign up at [mumo.chat](https://mumo.chat) and create a platform key at [Settings → API Keys](https://mumo.chat/settings/api-keys) (keys start with `mmo_live_`)
2. Export it in your shell before launching Cursor:
   ```bash
   export MUMO_API_KEY=mmo_live_YOUR_KEY_HERE
   ```
3. Restart Cursor so the new env var is picked up

## Using the panel

**Reliable invocation: ask explicitly.** The plugin ships a Cursor rule + an auto-triggering skill that keep the panel available to the agent on every conversation, but Cursor's rule system treats both as soft priors — the agent often chooses to answer directly on confident-looking questions even when the rule matches. The most reliable way to route a decision through mumo is an explicit ask:

- "Ask mumo about…"
- "Run this by a mumo panel"
- "Get me a second opinion from mumo on…"
- "What do different models think about…"

For the agent's decision criteria (what should go through mumo vs. what shouldn't), see [`skills/mumo/SKILL.md`](skills/mumo/SKILL.md).

### Try it first

> Ask mumo: Postgres or MongoDB for our event store given 50k events/day, a Postgres-experienced team, and a 3-month runway? What would we regret 6 months in?

The first round returns each model's prose plus a cross-model claim map showing where the panel agrees and where it splits. You can stop there, or `append_round` with typed snippets (KEEP / EXPLORE / CHALLENGE / CORE / SHIFT) to steer further.

See [mumo.chat/mcp](https://mumo.chat/mcp) for the tool reference, the deliberation loop, and prompt patterns. The canonical trigger language lives in [`skills/mumo/SKILL.md`](skills/mumo/SKILL.md).

## Links

- Product — https://mumo.chat
- MCP reference — https://mumo.chat/docs/mcp/reference
- REST API — https://mumo.chat/docs/api
- Claude Code / Cowork plugin — https://github.com/mumo-chat/mumo-mcp
- Issues — https://github.com/mumo-chat/mumo-cursor/issues

## License

MIT
