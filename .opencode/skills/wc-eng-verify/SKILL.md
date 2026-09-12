---
name: wc-eng-verify
description: White Crane verification gate via vendored QA skills. Use when testing backend changes, reviewing code, or backfilling API test suites.
---

# WC Eng Verify

Local vendored knowledge (offline, MIT):

- `.opencode/vendor/claude-skills/engineering-team/skills/senior-qa/SKILL.md` — test pyramid, coverage-gap analysis, E2E scaffolding. Extends the engineer's `expo lint / py_compile / pytest / curl /health` gate.
- `.opencode/vendor/claude-skills/engineering-team/skills/code-reviewer/SKILL.md` — PR analysis, quality metrics, antipattern detection. Run as pre-return review pass.
- `.opencode/vendor/claude-skills/engineering/skills/api-test-suite-builder/SKILL.md` — scans API routes → generates test suites. Use to backfill tests for `server.py` endpoints.
- `.opencode/vendor/claude-skills/engineering-team/skills/senior-fullstack/SKILL.md` — references + architecture docs only. AUDIT NOTE: scripts QUARANTINED (`_quarantined/senior-fullstack-scripts`, FAIL: env-var reads + runtime installs) — never execute them.

Rules: report failures, never silent pass. Coverage gaps in `/generate|/submit` paths are release blockers.
