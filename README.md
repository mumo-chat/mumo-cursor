# mumo — Cursor plugin

**Multi-model responses + cross-model reactions. Want more rounds? Context carries automatically. Stop when you have what you need.**

Claude, GPT, Gemini, Grok, Qwen, GLM, Kimi in parallel. For contested decisions — architecture, plan review, strategy — where a single model might be confidently wrong.

Works in Cursor 2.5+. For Claude Code and Cowork, see [`mumo-chat/mumo-mcp`](https://github.com/mumo-chat/mumo-mcp). For VS Code (GitHub Copilot), see [`mumo-chat/mumo-vscode`](https://github.com/mumo-chat/mumo-vscode).

---

![Cursor agent invokes the mumo panel via MCP](https://github.com/mumo-chat/mumo-cursor/raw/main/assets/screenshot-cursor-invocation.png)

*Invoke mumo from Cursor's agent chat — the agent routes through MCP, convenes the panel, and returns the models' synthesis inside Cursor.*

![The mumo claim map — what every model agreed on, challenged, or flagged for deeper exploration](https://github.com/mumo-chat/mumo-cursor/raw/main/assets/screenshot-claim-map.png)

*The claim map at [mumo.chat](https://mumo.chat) — each contested statement plus every model's reaction (keep, challenge, explore, core, shift). Not a consensus answer; the structure underneath it.*

![A multi-round mumo session carrying context forward across rounds](https://github.com/mumo-chat/mumo-cursor/raw/main/assets/screenshot-rounds.png)

*Context carries forward automatically — add follow-up rounds until you have what you need, without re-pasting anything.*

---

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

**Name `mumo` explicitly in your prompt.** The plugin ships a Cursor rule + an auto-triggering skill that keep the panel available to the agent on every conversation, but Cursor's rule system treats both as soft priors. Two things go wrong without explicit naming:

1. The agent may answer directly rather than route to mumo at all.
2. Even when it does route out, vague phrasings like "ask a panel" or "what do different models think" may resolve to a generic response or a different tool — not this MCP server.

**Reliable invocations** (the word `mumo` is what makes the routing stick):

- "Ask mumo about…"
- "Run this by a mumo panel"
- "Get me a second opinion from mumo on…"
- "What does mumo think about…"

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
