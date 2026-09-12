---
name: wc-eng-ops
description: White Crane dependency and performance operations. Use when managing Python/JS pins, profiling inference latency, or handling secrets and env hygiene.
---

# WC Eng Ops

Local vendored knowledge (offline, MIT):

- `.opencode/vendor/claude-skills/engineering/skills/dependency-auditor/SKILL.md` — multi-language scan, license compliance, upgrade planner. Use for torch/diffusers/transformers pin changes (stdlib-only `dep_scanner.py`, `license_checker.py` verified working).
- `.opencode/vendor/claude-skills/engineering/skills/performance-profiler/SKILL.md` — Python profiling + load testing. Use for `/generate` latency work on 12GB VRAM boxes.
- `.opencode/vendor/claude-skills/engineering-team/skills/senior-secops/SKILL.md` — SKILL.md + references only. AUDIT NOTE: scripts QUARANTINED (`_quarantined/senior-secops-scripts`, FAIL: exec/eval) — env/`HF_TOKEN` hygiene guidance only, never execute.

Rules: never `pip install` into the shared ComfyUI venv without approval. Document every pin change with before/after `health` + test-gen timings.
