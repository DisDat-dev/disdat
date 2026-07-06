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

## Use

Record with the Dis Dat extension, paste the capture at your agent, and say **"use the Dis Dat skill."**
The skill is optional — a Dis Dat paste is self-describing — it just makes consumption consistent.

The skill lives in [`skills/disdat/SKILL.md`](skills/disdat/SKILL.md).
