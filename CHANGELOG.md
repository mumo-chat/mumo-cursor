# Changelog

## 0.13.1 — 2026-11-21

- New listing description.
- README: install from inside Cursor with Customize → + Add → From GitHub Repository; the clone command stays as the terminal alternative.

## 0.13.0 — 2026-11-21

- The plugin now includes the mumo MCP server (`mcp.json`), so one install brings the server, the skill and the rule. The server connects with a browser sign-in: under Customize → MCPs, click **Authenticate** on `mumo`.
- If you added a `mumo` server by hand for an earlier version, remove it so you do not have two: delete the `mumo` entry from `~/.cursor/mcp.json` and reload the window.
- The **Add to Cursor** link at mumo.chat/install/cursor stays for adding the server without the plugin, and the API key path stays for Cursor's command-line agent and unattended runs.
- `SKILL.md` setup text and the README updated to match.

## 0.12.0 — 2026-11-21

- Setup now uses a browser sign-in. The **Add to Cursor** link at mumo.chat/install/cursor adds the server at `https://mumo.chat/api/mcp/oauth` with no API key; you then sign in from the `mumo` row under Tools & MCP. The tools, billing and limits are the same.
- A server already set up with an API key keeps working. To switch, remove it under Tools & MCP and add the sign-in server. The key path stays for Cursor's command-line agent and unattended runs; the README has both.
- The plugin still ships no MCP server of its own.
- `SKILL.md` setup text and the README updated to match.

## 0.11.0 — 2026-11-21

Coordinated client release rendered from the mumo-mcp 0.9.0 baseline. It goes with mumo MCP server 1.4.0.

- `wait_for_round` results are text only. The second line is `Action: <value>`, with the same five values as before, and the skill now tells the agent to branch on that line. The third line names the session and round. Under the header, each model that failed, was cut off, hit a cap or was cut short by its provider gets an `Issue:` line.
- New section, "Hand the wait to a subagent". Where the client supports subagents, the agent creates the round itself and has a subagent on the same model wait and read. The subagent hands back the round without the full answers, the points no model reacted to, and a short note on tone. It hands back only a finished round, and says so when it could not read the whole result.
- Refusals start with `Error: <code>`.
- `references/operating-notes.md` and `references/synthesis.md` updated to match.

## 0.10.0 — 2026-11-20

Coordinated client release rendered from the mumo-mcp 0.8.0 baseline.

- The skill says "reactions" where it said "claim map", and calls the moderator's quote-plus-type items "your reactions". The `append_round` parameter is still `snippets`, and the `claim_map` field keeps its name.
- Reading the reactions: the read tools print reacted-to claims in four groups. Contested (a model challenged it), Open (a model wants it explored), Pivotal (a model called it central, or it changed a model's frame) and Kept (a model kept it). Agreement between models is not verification.
- The loop: summarize each round for the user and agree before running another. `append_round` needs a `prompt`. Wait with both the `session_id` and the `round_id`, and reuse the returned `round_id` after a timeout. An identical `append_round` call within 24 hours returns the first acknowledgement, so reword the prompt to retry a failed round.
- A family name (`"claude"`, `"gpt"`, `"gemini"`) works as a model. `list_models` is sorted by what your plan can run.
- Request a Takeaway only when the user asks for one.
- `detail: "compact"` on `wait_for_round` and `get_session` shortens long sessions.
- Reference files renamed: `references/claim-maps.md` is now `references/reading-reactions.md`, and `references/snippets.md` is now `references/writing-reactions.md`. The Confidence scores section is removed, because the tools do not print those fields.
- README updated to match.

## 0.9.0 — 2026-11-19

Coordinated client release rendered from the mumo-mcp 0.7.0 baseline.

- New tool: `ask_a_model` puts a question to one model of your choice and returns one answer, with no cross-model reactions or claim map. `SKILL.md` has a new section on when to use it and when to use `create_deliberation` instead. The plugin sets no tool allowlist, so the tool is available as soon as the server lists it.
- `SKILL.md` now tells the agent to surface each round's `web_url`. `claim_map_url` still works and carries the same value.
- README tool list updated.

## 0.8.0 — 2026-09-09

Installable again from a folder, and honest install instructions.

- New `.cursor-plugin/marketplace.json` listing this repo as a one-plugin marketplace (`source: "./"`). Cursor's add-from-folder flow requires a marketplace manifest even for a single plugin; without it the add fails with "No marketplace manifest found", which is what 0.7.0 did.
- README install step 2 now describes the two sideload paths that exist — `~/.cursor/plugins/local/mumo` + Developer: Reload Window, or adding the cloned folder from the Plugins view — instead of a "Cursor: Add Plugin from GitHub" command that does not exist for individual accounts.
- `plugin.json`: `repository` is a plain URL, `author.email` added.
- Still no bundled MCP server, now with the reason on record: Cursor plugin variables (`${MUMO_API_KEY}` in `mcp.json`) are set in the team dashboard, and an individual sideload install never prompts for the value (verified 2026-09-09), so a bundled server would ship with no credential. The bundled, key-free server returns when the mumo MCP endpoint supports OAuth.
- Skill, rule, tool list, and behavior unchanged from 0.7.0.

## 0.7.0 — 2026-09-08

Coordinated client release rendered from the mumo-mcp 0.6.0 baseline.

- `SKILL.md`: `wait_for_round` taught in its one-id form (`session_id` alone resolves to the round in flight; `round_id` only for an earlier round); intro names the current model families and the typed cross-model reactions.
- `references/operating-notes.md` re-synced with the baseline (the `/progress` diagnostic-polling section).
- README intro updated.

## 0.6.0 — 2026-07-10

Install architecture: one-click server config, no environment variables.

- The plugin no longer contributes an MCP server. Add the mumo server with the official "Add to Cursor" link at [mumo.chat/install/cursor](https://mumo.chat/install/cursor) (or merge the equivalent `~/.cursor/mcp.json` entry shown there). The previous env-var path (`MUMO_API_KEY`) was unreliable for GUI-launched Cursor — especially on macOS, where the variable didn't survive reboots — and is retired.
- Skill and rule are unchanged in behavior: they bind to mumo's tool names, so they work with the server you configure regardless of its label.
- Tool list updated: `share_session` (publish a session at a public link) joined the server's toolset.

## 0.5.0 — 2026-07-04

Reaction-visibility model + per-round Takeaway.

- New "Reactions are in the ledger, responses are on the record" section: participants never see each other's reactions — each model weaves its own prior reactions into its next response, and the claim map is the moderator's privileged preview. Moderator snippets are the one path that puts ledger content directly in front of the panel.
- Pacing note in "When to continue": positions develop across rounds — a reaction gets woven into the next round's prose, which draws reactions of its own a round later.
- Takeaway: `takeaway: true` (create + append) generates `round_takeaway` (`bottom_line` + `items[]`), the per-round summary. New `references/takeaway.md` (replaces `references/recap.md`).

## 0.4.0 — 2026-06-19

Skill triggering + prompt-voice updates.

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
