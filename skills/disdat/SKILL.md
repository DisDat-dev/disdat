---
name: disdat
description: >-
  Consume a Dis Dat capture. When the user pastes a "DIS DAT · capture" block (a recorded reaction to a
  live web page plus re-hosted artifact URLs), use this to fetch the right artifacts and make grounded
  code changes from the exact captured values — instead of guessing or re-rendering the site.
---

# Dis Dat — capture skill

[Dis Dat](https://disdat.dev) is a browser extension that records what a web page **actually renders** plus the
user's **spoken reaction**, and hands you a precise, re-hosted bundle. This skill tells you how to consume it.

## When to use

Trigger this whenever the user's message contains a **Dis Dat capture** — a block headed
`━━━ DIS DAT · capture for your coding agent ━━━`, or the user says *"use the Dis Dat skill"* / pastes a
`disdat-*` artifact URL (litterbox / 0x0 / tmpfiles `.gz`). The block carries the user's reaction and a
menu of downloadable artifacts.

## How a capture is shaped

The pasted block contains:
- The user's **reaction**, transcribed and crunched into short "code-speak" — *this is the intent.* Treat
  their words as what they want; the captured facts are the ground truth to build it from.
- A **menu** of artifacts, each a **gzipped JSON** at its own expiring URL. Fetch only what the task needs:

| artifact | holds | grab when |
|---|---|---|
| `core` | code-speak + **full transcript** + engaged elements (verbatim computed **and authored** CSS incl `:hover`/pseudo, html, React props/state, listeners, animations, WebGL shaders+uniforms+a rendered frame) + observed network/API + persisted state + `<head>` + a causal timeline | **always — start here** |
| `dom` | the full document tree (structure/order/attrs) | rebuilding whole-page structure |
| `screenshots` | time-ordered JPEG timeline of what the user saw | you need to *see* it / visual diff |
| `assets` | re-hosted image/font/SVG **bytes** (base64) | a pixel-exact clone (avoids 403s) |
| `sources` | the page's own first-party JS — the render harness behind the WebGL/animations | replicate behaviour / self-host the real thing |

## How to consume

1. **Always fetch `core` first.** It is small and has the transcript, the elements with exact values, and
   the API/state. Most tasks need only `core`.
2. **Fetch more only as the task demands**, per the menu's recommendation:
   - *clone the look* → `core` + `assets` + `screenshots`
   - *rebuild behaviour / motion / WebGL* → `core` + `sources` + `dom`
   - *wire the data* → `core` (already has network + state)
3. Each artifact is gzip, not a web page. Fetch + gunzip:
   - shell: `curl -s "<url>" | gunzip`
   - JS: `await new Response((await fetch(url)).body.pipeThrough(new DecompressionStream('gzip'))).json()`
4. **Build from the EXACT captured values.** Every element carries its real rect, computed + authored CSS,
   and (for canvas) the actual shaders/uniforms/frame. Reproduce from these — do **not** re-render the live
   site, do **not** invent values, do **not** assume what's under the cursor was the target (the user's words
   carry the intent; the cursor heatmap is only a loose hint).
5. Ratios = `rect ÷ meta.viewport`. Screenshots are a visual reference, not a ruler — trust the CSS values.

## Notes

- Secrets are redacted and cookies skipped before upload, so `state`/`network` are safe but may show
  `⟦redacted⟧` where a token was.
- Artifact URLs **expire** (hours to weeks). Fetch them in the same session the user pastes them.
- If an artifact 404s it has expired — ask the user to re-capture.
- This skill is optional: a Dis Dat paste is self-describing and you can act on it without the skill. The
  skill just makes consumption consistent and reliable.
