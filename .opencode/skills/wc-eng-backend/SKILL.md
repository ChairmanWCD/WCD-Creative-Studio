---
name: wc-eng-backend
description: White Crane backend engineering via vendored role skills. Use when designing FastAPI endpoints, hardening the inference API contract, or scaffolding backend code.
---

# WC Eng Backend

Local vendored knowledge (offline, MIT):

- `.opencode/vendor/claude-skills/engineering-team/skills/senior-backend/SKILL.md` — REST/GraphQL API design, auth, microservice patterns, load testing. Primary reference for FastAPI inference-server work (`server.py` + `app.py` upstream in White Crane Generation, external to this repo).
- `.opencode/vendor/claude-skills/engineering/strict-api/SKILL.md` — REST linter + breaking-change detector. Consult before changing `/submit|/generate|/status|/result|/cancel` shapes.
- `.opencode/vendor/claude-skills/engineering-team/skills/senior-frontend/SKILL.md` — React patterns/state (transfers to Expo RN UI). AUDIT NOTE: WARN — runtime package installation; read SKILL.md + references freely, run its scripts only after review.
- `.opencode/vendor/claude-skills/engineering/zero-hallucination-coder/skills/zero-hallucination-coder/SKILL.md` — mandatory working loop: Discuss → Map → Decompose → Execute → Verify.

Rules: never break the `Input/Output` contract in `app.py:40-80`. Keep DESIGN.md tokens intact in UI work.
