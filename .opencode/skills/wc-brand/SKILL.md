---
name: wc-brand
description: White Crane brand and DESIGN.md design systems. Use when choosing colors, typography, tokens, or enforcing brand contract.
---

# WC Brand

Local knowledge — kept as-is, no copy:

- `.opencode/vendor/open-design/design-systems/` — 150+ systems, each `DESIGN.md` + `tokens.css` + `design-tokens.json` + `components.html`
- `.opencode/vendor/open-design/craft/` — `typography.md`, `color.md`, `anti-ai-slop.md`, `accessibility-baseline.md`, `laws-of-ux.md`
- `.opencode/vendor/open-design/skills/` relevant: `brand-extract`, `design-md`, `brandkit`, `brand-guidelines`

Workflow:
1. Pick closest system or read existing project `DESIGN.md` as source of truth.
2. Apply tokens first, then craft rules. Brand wins over generic craft on conflict.
3. Emit token deltas + component notes for handoff.
