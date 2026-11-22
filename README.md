# mumo — Cursor plugin

**Multi-model responses + cross-model reactions. Want more rounds? Context carries automatically. Stop when you have what you need.**

Claude, GPT, Gemini, Grok, DeepSeek, Kimi, and more, in parallel. Each model answers in full and then reacts to the others' claims in its own words — typed cross-model reactions, not a merged verdict — for architecture, plan review, pricing, and strategy calls worth checking across labs.

Works in Cursor 2.5+.

---

![Cursor agent invokes the mumo panel via MCP](https://github.com/mumo-chat/mumo-cursor/raw/main/assets/screenshot-cursor-invocation.png)

*Invoke mumo from Cursor's agent chat — the agent routes through MCP, convenes the panel, and returns the models' synthesis inside Cursor.*

![The models' reactions in mumo — what every model agreed on, challenged, or flagged for deeper exploration](https://github.com/mumo-chat/mumo-cursor/raw/main/assets/screenshot-claim-map.png)

*The models' reactions at [mumo.chat](https://mumo.chat) — each contested statement plus every model's reaction (keep, challenge, explore, core, shift). Not a consensus answer; the structure underneath it.*

![A multi-round mumo session carrying context forward across rounds](https://github.com/mumo-chat/mumo-cursor/raw/main/assets/screenshot-rounds.png)

*Context carries forward automatically — add follow-up rounds until you have what you need, without re-pasting anything.*

---

## What's in the box

- **MCP server** — `mcp.json` points Cursor at `https://mumo.chat/api/mcp/oauth`, connected with a browser sign-in. Tools: `create_deliberation`, `ask_a_model`, `wait_for_round`, `append_round`, `get_session`, `share_session`, `list_sessions`, `list_models`, `get_credit`.
- **Auto-triggering skill** — `skills/mumo/SKILL.md` tells the agent *when* to reach for the panel (architecture tradeoffs, plan reviews, contested decisions)
- **Cursor rule** — `rules/mumo.mdc` reinforces routing on contested decisions

## Install

**1. Install the plugin.** Clone it into Cursor's local plugins folder and reload:

```bash
git clone https://github.com/mumo-chat/mumo-cursor ~/.cursor/plugins/local/mumo
# then in Cursor: Command Palette → "Developer: Reload Window"
```

Open **Customize** and search for `mumo`: the plugin shows one MCP server, one skill and one rule.

**2. Sign in.** Under **Customize → MCPs**, the `mumo` server reads **Needs Authentication**. Click **Authenticate**. Your browser opens to **Connect your mumo account**. Sign in to mumo if asked (or create an account; every account gets free credit each month), click **Continue**, then **Allow access**. The pages list `www.cursor.com` and an address on this computer (`localhost`) as where you are sent next: that is Cursor waiting for the sign-in to finish. Accept the browser's **Open Cursor?** prompt. The `mumo` server then shows green with its tools enabled.

To disconnect later, remove the connection under **Settings → API Keys → Connected apps** at [mumo.chat/settings/api-keys](https://mumo.chat/settings/api-keys).

### Only want the server?

To add the mumo tools without the skill and rule, click **Add to Cursor** at [mumo.chat/install/cursor](https://mumo.chat/install/cursor), or merge this entry into `~/.cursor/mcp.json` by hand, then sign in as above. Skip this if you installed the plugin.

```json
{
  "mcpServers": {
    "mumo": { "url": "https://mumo.chat/api/mcp/oauth" }
  }
}
```

### Using an API key instead

For Cursor's command-line agent or an unattended run, without the plugin: create a key at [Settings → API Keys](https://mumo.chat/settings/api-keys) (keys start with `mmo_live_`) and paste it into the key section of [mumo.chat/install/cursor](https://mumo.chat/install/cursor). It builds an install link for the key server:

```json
{
  "mcpServers": {
    "mumo": {
      "url": "https://mumo.chat/api/mcp",
      "headers": {
        "Authorization": "Bearer mmo_live_YOUR_KEY_HERE"
      }
    }
  }
}
```

Keys are revocable any time from the dashboard; never commit a file that holds a live key.

Full walkthrough: [mumo.chat/install/cursor](https://mumo.chat/install/cursor).

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

The first round returns each model's response plus the models' reactions to each other's claims, showing where the panel agrees and where it splits. You can stop there, or `append_round` with your own typed reactions (KEEP / EXPLORE / CHALLENGE / CORE / SHIFT) to steer further.

See [mumo.chat/install/cursor](https://mumo.chat/install/cursor) for the install guide and [mumo.chat/docs/mcp](https://mumo.chat/docs/mcp) for the tool reference. The canonical trigger language lives in [`skills/mumo/SKILL.md`](skills/mumo/SKILL.md).

## Links

- Product — https://mumo.chat
- Install guide — https://mumo.chat/install/cursor
- MCP reference — https://mumo.chat/docs/mcp
- REST API — https://mumo.chat/docs/api
- Issues — https://github.com/mumo-chat/mumo-cursor/issues

## License

MIT
