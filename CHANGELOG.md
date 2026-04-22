# Changelog

## 0.1.5 — 2026-04-22

- Add `assets/logo.png` (512×512, dark background plate) — mumo's canonical square icon, reused from the brand kit. Required for Cursor Marketplace submission.
- `plugin.json`: reference `assets/logo.png` via the `logo` field.

## 0.1.4 — 2026-04-22

- README + rule: require the word `mumo` in invocations. Smoke-testing 0.1.3 surfaced that ambiguous phrasings like "ask a panel" can route to a generic response instead of this plugin — the agent resolves "panel" to a generic experts pattern rather than our MCP server.
- `rules/mumo.mdc` trigger list: replaced "what do different models think / get me a panel" with mumo-explicit phrasings.

## 0.1.3 — 2026-04-22

- README: honest note that Cursor auto-trigger is best-effort. Smoke-tested v0.1.2 and confirmed the rule loads into the agent's context but is overridden by Cursor's direct-answer bias on contested-looking prompts. The rule + skill still nudge the agent's reasoning; explicit invocation ("ask mumo…", "run this by a panel") is the reliable primary UX.

## 0.1.2 — 2026-04-22

- Strengthen `rules/mumo.mdc` copy. 0.1.1 used permissive "reach for mumo" language and the agent weighted its own priors over the rule. 0.1.2 uses imperative framing ("call create_deliberation OR ask the user") and "before giving a direct answer" precedence. Trade-off: more likely to trigger, slightly pushier UX.

## 0.1.1 — 2026-04-22

- `rules/mumo.mdc` — Cursor rule with `alwaysApply: true` so the agent proactively considers the panel on every conversation. Cursor doesn't auto-trigger skills on description match the way Claude Code does; rules are Cursor's equivalent primitive. Short body (~180 words) to keep per-turn token cost low.

## 0.1.0 — 2026-04-22

Initial release.

- `.cursor-plugin/plugin.json` manifest (MIT, Sourced LLC, homepage https://mumo.chat/mcp)
- `mcp.json` wiring remote streamable-HTTP MCP server at https://mumo.chat/api/mcp with `${env:MUMO_API_KEY}` bearer token
- `skills/mumo/SKILL.md` — auto-triggering skill, shared canonical source with [`mumo-chat/mumo-mcp`](https://github.com/mumo-chat/mumo-mcp)
