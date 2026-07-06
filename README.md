# Skillify any webpage — the Dis Dat skill

The [Dis Dat](https://disdat.dev) capture skill for AI coding agents. Teaches your agent to consume a
Dis Dat capture (a recorded reaction to a live web page + re-hosted artifact bundle) and make grounded
code changes from the exact captured values.

## Install

```
npx skills add DisDat-dev/disdat
```

Works with Claude Code, Cursor, GitHub Copilot, Cline and 18+ agents (via [skills.sh](https://skills.sh)).

Or install manually — tell your agent: *"Fetch https://api.disdat.dev/skill.md and save it as my Dis Dat
skill."*

## Skillify any webpage

Tell your agent **"skillify https://site-you-love.com"** — it captures the page's real design layers
on your machine (authored CSS incl. hover states, exact rects, animation curves, WebGL shaders — not
a screenshot, not scraped text), then hands you the design as **knobs**: every node named, its current
value, its meaningful range, and a multiple-choice taste menu. You art-direct by answering questions —
*"springy overshoot, softer, snappier, or editorial-flat?"* — and it rebuilds at that level, in YOUR
brand. You don't get a copy of their page; you get the skill of the developer who built it.

```bash
node skills/disdat/scripts/skillify.mjs https://example.com --select ".hero" --sources
```

No account, nothing uploaded. For **human** captures — your voice + your cursor, across pages,
behind logins — get [the extension](https://disdat.dev) (30 free min).

## Use

Record with the Dis Dat extension, paste the capture at your agent, and say **"use the Dis Dat skill."**
The skill is optional — a Dis Dat paste is self-describing — it just makes consumption consistent.

The skill lives in [`skills/disdat/SKILL.md`](skills/disdat/SKILL.md).
