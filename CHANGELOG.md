# Changelog

## 0.4.0 — 2026-06-19

Coordinated release — all clients aligned on 0.4.0. Skill triggering + prompt-voice updates (Trello #245), rendered from the mumo-mcp baseline.

- Triggering: dropped the "contested" gate — the description now leads with pre-implementation review (esp. anything touching auth, security, tokens, payments, data exposure, or migrations), not "contested decisions only."
- Author-bias counter (When to use): if you authored the plan or code under review, that's a reason FOR a panel — the author is the worst-positioned reviewer of their own work.
- New "Prompt voice" section: write the prompt first-person as the operator, not "You are X" case-study framing.
- Surface `claim_map_url` after each round so the user can open the claim map directly.

## 0.2.4 — 2026-05-22

README update.

## 0.2.3 — 2026-05-22

Sync with the canonical baseline plus a marketplace-listing alignment. Bundles two prior unpushed commits (`52ed422` description tokenization, `c94a4ac` references rename + `recap.md`) under a single release marker. Triggered by Cursor's `review-plugin-submission` audit catching a broken `references/recap.md` link.

- **`plugin.json` description** restored to the v0.1.6 marketplace hero ("Multi-model responses + cross-model reactions. Want more rounds? Context carries automatically. Stop when you have what you need."). Current README hero already matches. A post-v0.1.6 change to `plugin.json` drifted away from it without a corresponding CHANGELOG entry — this restores continuity.
- **`skills/mumo/SKILL.md` description** rewritten with WHAT+WHEN phrasing per the AgentSkills-style validator hint: "Runs structured multi-model deliberations ... Use when independent perspectives are needed on ...". The new description applies broadly to the agent reading the skill; the marketplace listing description (above) stays punchy for the listing surface.
- **`skills/mumo/reference/` → `skills/mumo/references/`** rename (plural, matching the SKILL.md link targets). Previously SKILL.md linked to `references/snippets.md` etc. while the dir was at `reference/snippets.md`, breaking all 6 ref-doc links at follow-time. Caught while fixing the recap.md miss.
- **`skills/mumo/references/recap.md` added** — covers the `recap_round` / `recap_session` opt-in flags surfaced in the recent mumo MCP server iteration. Caught by Cursor's `review-plugin-submission` audit as the P0 blocker.
- **`plugin.json` version** 0.2.2 → 0.2.3.

Note: the v0.2.1 entry below still references the old `skills/mumo/reference/` (singular) path. Preserved as historical record of what was true at that release; the v0.2.3 rename is documented here, not retconned there.

## 0.2.2 — 2026-05-05

Removed `agents/mumo-moderator.md`. The agent positioned itself as the moderator role, but moderation is exactly what should stay with the primary agent — it owns the conversational context, the user's intent, and the cross-round steering decisions. A subagent that ran a "complete brief" panel was a thin operational helper at best, and the framing risked the primary agent over-delegating.

- `agents/` directory removed.
- README updated to drop the moderator subagent line.

## 0.2.1 — 2026-05-05

Architecture rewrite: kernel + playbooks + reference, plus a moderator subagent. Cursor lacks the keychain/userConfig flow Claude Code has, so the env-var setup stays canonical.

- `skills/mumo/SKILL.md` — rewritten as a compact kernel: deliberation loop, snippet-as-attention doctrine, non-forwarding test, continuation/stop rules, playbook index, user-preferences section. Synthesis guidance deferred to reference.
- `skills/mumo/playbooks/` — four cognitive-shape playbooks: `contested-decision`, `design-review`, `uncertainty-expansion`, `red-team`. Loaded at most one per session when the shape clearly fits.
- `skills/mumo/reference/` — five reference docs: `claim-maps`, `snippets`, `model-selection`, `synthesis`, `operating-notes`. Loaded on demand for extended mechanics.
- `agents/mumo-moderator.md` — new subagent for running deliberations in isolated context. Defers to whichever model the user is currently engaged with in Cursor (no model pin).
- `rules/mumo.mdc` — unchanged; reinforces routing on contested decisions.
- `wait_for_round` added to README tool list (was missing since 0.1.8).
- `plugin.json` author updated to `mumo`, homepage to `/install/cursor`, version bumped to 0.2.1.

## 0.1.8 — 2026-04-24

- Added `get_credit` as the sixth MCP tool (wallet balance + bucket breakdown + autorefill state). README and SKILL.md tool map updated.

## 0.1.7 — 2026-04-23

Skill + rule content update, no runtime behavior change. Mirrors the MCP doc demotion shipping on mumo.chat.

- `skills/mumo/SKILL.md` — dropped "Modes" + "Surfacing to humans", stopped passing `rounds: 1`, renamed the `get_session` tool-map row.
- `skills/mumo/SKILL.md` — fixed two Cursor localization bugs: `"Restart Claude Code"` → `"Restart Cursor"`, and `application: "Claude Code"` → `application: "Cursor"`.
- `rules/mumo.mdc` — dropped `rounds: 1` from the firing hint.

## 0.1.6 — 2026-04-22

Listing refresh, no runtime behavior change.

- Manifest `description` now leads with the cross-model-reactions + iterative-rounds value prop: *"Multi-model responses + cross-model reactions. Want more rounds? Context carries automatically. Stop when you have what you need."*
- README gained three screenshots below the hero: Cursor agent chat invoking mumo, the mumo.chat claim map, and a multi-round session showing context carried forward. Absolute GitHub raw URLs so the listing renders the same on GitHub, the Cursor marketplace, and any social share.
- Kept the "Name `mumo` explicitly in your prompt" section — it's load-bearing for Cursor's soft-prior rule system.

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

- `rules/mumo.mdc` — Cursor rule with `alwaysApply: true` so the agent proactively considers the panel on every conversation. Short body (~180 words) to keep per-turn token cost low.

## 0.1.0 — 2026-04-22

Initial release.

- `.cursor-plugin/plugin.json` manifest (MIT, Sourced LLC, homepage https://mumo.chat/mcp)
- `mcp.json` wiring remote streamable-HTTP MCP server at https://mumo.chat/api/mcp with `${env:MUMO_API_KEY}` bearer token
- `skills/mumo/SKILL.md` — auto-triggering skill.
