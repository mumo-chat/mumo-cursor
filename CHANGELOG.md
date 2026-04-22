# Changelog

## 0.1.1 — 2026-04-22

- `rules/mumo.mdc` — Cursor rule with `alwaysApply: true` so the agent proactively considers the panel on every conversation. Cursor doesn't auto-trigger skills on description match the way Claude Code does; rules are Cursor's equivalent primitive. Short body (~180 words) to keep per-turn token cost low.

## 0.1.0 — 2026-04-22

Initial release.

- `.cursor-plugin/plugin.json` manifest (MIT, Sourced LLC, homepage https://mumo.chat/mcp)
- `mcp.json` wiring remote streamable-HTTP MCP server at https://mumo.chat/api/mcp with `${env:MUMO_API_KEY}` bearer token
- `skills/mumo/SKILL.md` — auto-triggering skill, shared canonical source with [`mumo-chat/mumo-mcp`](https://github.com/mumo-chat/mumo-mcp)
