# Dis Dat — agent skill

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

## Headless capture included

The skill bundles a capture script — your agent can study any URL by itself, on your machine:

```bash
node skills/disdat/scripts/capture.mjs https://example.com --select ".hero" --sources
```

Real layers out: computed **and authored** CSS (incl. pseudo rules), exact rects, animations, WebGL
shaders/uniforms, DOM outline, screenshots, first-party sources. No account, nothing uploaded.
For **human** captures — your voice + pointing, across pages — get [the extension](https://disdat.dev).

## Use

Record with the Dis Dat extension, paste the capture at your agent, and say **"use the Dis Dat skill."**
The skill is optional — a Dis Dat paste is self-describing — it just makes consumption consistent.

The skill lives in [`skills/disdat/SKILL.md`](skills/disdat/SKILL.md).
