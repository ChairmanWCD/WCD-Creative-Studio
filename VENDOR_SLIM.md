# VENDOR_SLIM.md — what was copied from White Crane Studio and what was left out

Source (untouched): `C:\Users\Ken Bai\White Crane LLC\White Crane Studio\.opencode\vendor` — 267.6 MB, 12,630 files.

## Copied (slim subset, referenced by opencode.json + studio-designer)

- `open-design/skills/` (~3 MB) — frontend-design, imagegen-*, brand-extract, design-md, design-review, etc.
- `open-design/design-templates/` (~36 MB) — web-prototype, mobile-app, dashboard, saas-landing, decks
- `open-design/craft/` — typography, color, anti-ai-slop
- `open-design/prompt-templates/` — image/ + video/ prompt library
- `open-design/design-systems/` (~30 MB) — `*/DESIGN.md`, `tokens.css`, `design-tokens.json` (brand contract)
- `claude-skills/engineering-team/skills/` — senior-backend/frontend/fullstack/qa/secops, code-reviewer
- `claude-skills/engineering/strict-api/` — API reality check
- `claude-skills/engineering/zero-hallucination-coder/skills/` — Discuss→Map→Decompose→Execute→Verify
- `claude-skills/engineering/skills/` — api-test-suite-builder, dependency-auditor, performance-profiler, skill-security-auditor

## Left out (heavy / not referenced)

- `open-design/apps/` (~70 MB), `plugins/` (~66 MB), `docs/` (~17 MB), `assets/` (~17 MB), `data/`, `tools/`, `e2e/`, `packages/`, `deploy/`, `.github/`
- `.opencode/node_modules/` (reinstall via npm if needed)

## Resync full vendor (optional)

```powershell
$src = "C:\Users\Ken Bai\White Crane LLC\White Crane Studio\.opencode\vendor"
$dst = "C:\Users\Ken Bai\White Crane LLC\White Crane Lab\Project repo\WCD Creative Studio\.opencode\vendor"
Copy-Item -LiteralPath $src -Destination $dst -Recurse -Force
```

Never execute `_quarantined/` scripts (auditor FAIL). `senior-frontend` scripts are WARN — review before running.
