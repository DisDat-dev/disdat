---
name: disdat
description: >-
  Consume a Dis Dat capture. When the user pastes a "DIS DAT · capture" block (a recorded reaction to a
  live web page plus re-hosted artifact URLs), use this to fetch the right artifacts and — by default —
  STUDY what was captured: decompose it into a node graph of techniques and knobs, learn every layer at
  full depth, and rebuild it in the user's own brand. Never ship a verbatim copy of someone else's page.
---

# Dis Dat — capture skill

[Dis Dat](https://disdat.dev) is a browser extension that records what a web page **actually renders** plus the
user's **spoken reaction**, and hands you a precise, re-hosted bundle. This skill tells you how to consume it.

## When to use

Trigger this whenever the user's message contains a **Dis Dat capture** — a block headed
`━━━ DIS DAT · capture for your coding agent ━━━`, or the user says *"use the Dis Dat skill"* / pastes a
`disdat-*` artifact URL (litterbox / 0x0 / tmpfiles `.gz`).

## How a capture is shaped

The pasted block contains:
- The user's **reaction**, transcribed and crunched into short "code-speak" — *this is the intent.* Treat
  their words as what they want; the captured facts are the ground truth to work from.
- A **menu** of artifacts, each a **gzipped JSON** at its own expiring URL. Fetch only what the task needs:

| artifact | holds | grab when |
|---|---|---|
| `core` | code-speak + **full transcript** + engaged elements (verbatim computed **and authored** CSS incl `:hover`/pseudo, html, React props/state, listeners, animations, WebGL shaders+uniforms+a rendered frame) + observed network/API + persisted state + `<head>` + a causal timeline | **always — start here** |
| `dom` | the full document tree (structure/order/attrs) | rebuilding whole-page structure |
| `screenshots` | time-ordered JPEG timeline of what the user saw | you need to *see* it / visual diff |
| `assets` | re-hosted image/font/SVG **bytes** (base64) | pixel-exact reference (avoids 403s) |
| `sources` | the page's own first-party JS — the render harness behind the WebGL/animations | replicate behaviour / study the real thing |

Each artifact is gzip, not a web page:
- shell: `curl -s "<url>" | gunzip`
- JS: `await new Response((await fetch(url)).body.pipeThrough(new DecompressionStream('gzip'))).json()`

URLs **expire** (hours to weeks) — fetch in the same session. A 404 = expired; ask the user to re-capture.

## The default mode: STUDY, don't copy

Unless the page belongs to the user (their own product, their localhost), a Dis Dat capture is **study
material, not clip-art**. Your job is to extract the *skill of the developer who built it* — not to ship
their work. Work like this:

### 1. Quarantine anything verbatim
If you save any captured material as-is (shader source, CSS, assets, markup), put it in
`reference/disdat-<site>-<date>/` with a `PROVENANCE.md` stating the source URL, the capture date, and
that it is another site's work, kept **for study only**. Never place verbatim copies into the user's
product source tree.

### 2. Decompose it into a NODE GRAPH
Study the captured layers and write the thing up as a graph of named, editable **knobs** — the way its own
developer would think about it. The capture gives you the REAL values, so this is analysis, not guessing:

- **Layer stack** — enumerate every layer and how they composite: background image → gradient → WebGL
  morph shader → grain overlay → blend modes. There are usually MORE layers than it first appears; find
  them all. If it's 15 WebGL passes, it's 15 nodes — never collapse them.
- **Per-layer knobs** — for each node, name what varies: gradient stops, shader uniforms (and what each
  one actually does — read the shader source and the captured uniform values), spring
  constants/easings/durations, stagger offsets, palette, typographic scale, border-radius language,
  noise/grain amounts.
- **Interactions** — which knobs feed which (scroll position → uniform, hover → spring target), read from
  the captured listeners, animations, and the JS render harness in `sources`.
- **The feel** — motion character (overshoot, settle time, curve shape) is a knob too; capture the actual
  cubic-bezier/spring values, don't eyeball them.

### 3. Rebuild at the SAME depth, in the user's brand
Re-derive each node with the user's palette, brand, and content — same techniques, same number of layers,
same finesse. **Never dumb it down** into a cheap approximation just because a simpler version is easier.
The output is a *permutation at the original's level of craft*, not a copy: new values through the same
graph. The structure/technique is what was learned; every concrete value should be the user's.

### 4. Regenerate creative assets — never reuse them
Verbatim creative assets (photos, illustrations, figurines, logos, fonts without a license, copy text) are
**not fair game to ship**. Instead:
- Describe the asset in exhaustive detail from the capture (style, lighting, palette, composition,
  texture, mood) — you can see it in `screenshots`/`assets`.
- Regenerate an on-brand equivalent with whatever image tool is available: your own generation ability if
  you have one, or the user's API (Ideogram, OpenRouter, etc.) — **ask the user for the key/tool they
  want used** if none is configured.
- Same for Lottie/animation assets: learn the motion character and rebuild it.

### 5. Report the graph to the user
Tell them what you learned before or while building: *"Behind this section: N layers — A (knobs: …),
B (knobs: …), C. Here's your on-brand permutation, and here are the knobs you can now turn."* The user
should come away able to art-direct the thing — that's the product: **they gain the developer's skill,
not the developer's file.**

### When the page IS the user's own
If the capture is of the user's own product (their words or the URL make it clear), skip the quarantine —
act on their reaction directly and reproduce/fix from the exact captured values.

## Hard rules

- The user's words carry the intent; the cursor heatmap is only a loose hint — don't assume the element
  under the cursor is the target.
- Build from the EXACT captured values (rects, CSS, uniforms) — don't re-render the live site, don't
  invent values. Ratios = `rect ÷ meta.viewport`; screenshots are reference, not a ruler.
- Secrets are redacted and cookies skipped before upload; `⟦redacted⟧` marks where a token was.
- This skill is optional: a Dis Dat paste is self-describing and works without it. The skill makes
  consumption consistent — and makes the study-don't-copy default explicit.
