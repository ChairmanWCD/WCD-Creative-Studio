# WCD Creative Studio

Art-direction studio + local image engine in one repo. An `art-director` orchestrator routes briefs to `studio-designer` (visual + image generation) and `studio-engineer` (build + verify) — backed by vendored skill libraries and the built-in Z-Image inference server.

Renamed in place from `z-image-inference`. White Crane Studio source repo is untouched (read-only copy source).

## Workflow: server → agents → output

```
1. Setup engine        .\setup.ps1  →  .\start.ps1  →  GET /health = {ok}
2. Brief director      art-director clarifies audience/surface/brand, classifies design-only|build-ready|image-gen
3. Designer generates  POST /submit {prompt,image_size,seed} → poll /status → GET /result → save studio/dist/
4. Icons               mastericons MCP (lucide line, recolor to brand tokens)
5. Engineer verifies   contract intact, py_compile/pytest, curl /health, returns diff + logs
6. Director approves   brand contract holds + verify logs pass
```

Live server: `http://localhost:8100` (LAN legacy: `http://192.168.1.202:8100`). Docs: `/docs`. UI: `/`.

## Quickstart

1. One-time: `powershell -ExecutionPolicy Bypass -File .\setup.ps1`
2. Start: `.\start.ps1` (default port 8100, `LOCAL_MODELS_DIR=C:\Users\Ken Bai\ComfyUI-Shared\models`)
3. Health: `curl http://localhost:8100/health`
4. Open this folder in opencode (run opencode here). Quit + restart opencode after config change.
5. Brief: `art-director` routes to designer/engineer via Task. Designer owns image prompts (prompt + size + seed).
6. Sample: open `studio/dist/shiba-cafe-deck/index.html`.

## Layout

- `server.py` / `app.py` / `config.py` — Z-Image-Turbo async queue API (`/submit → /status → /result`, sync `/generate`, `/health`, `/api/history`, `/output/*`)
- `setup.ps1` / `start.ps1` / `requirements_zimage.txt` — Windows-native install + launcher (RTX 5070 12GB, CPU offload, `MAX_IMAGE_SIDE=1024`)
- `static/index.html` — web UI
- `opencode.json` — default_agent art-director, skills paths, localhost + LAN inference permissions, mastericons MCP
- `.opencode/agent/` — `art-director.md` (orchestrator, no edits), `studio-designer.md`, `studio-engineer.md`
- `.opencode/skills/wc-*` — brand, decks, ui, image-gen, eng-backend, eng-verify, eng-ops
- `.opencode/vendor/` — **slim subset** (see `VENDOR_SLIM.md`): open-design skills/design-templates/craft/prompt-templates/design-systems + claude-skills engineering. Full 267MB vendor stays in White Crane Studio; rsync if needed.
- `studio/dist/shiba-cafe-deck/` — sample workflow output (see below)

## Agents

- **art-director** (primary, edit/bash deny): clarify brief → classify → delegate via Task → reconcile with explicit deltas → approve on brand + logs.
- **studio-designer** (visual + gen): consults local vendor knowledge, `GET /health`, `POST /submit|/generate`, saves under `studio/dist/`, inlines lucide SVGs, returns files + prompts/seeds/sizes + handoff.
- **studio-engineer** (code): `zero-hallucination-coder` Discuss→Map→Decompose→Execute→Verify, `senior-qa`/`code-reviewer`/`api-test-suite-builder`, `dependency-auditor`/`performance-profiler`. Never executes `_quarantined/` scripts. Verifies with `py_compile`/`pytest` + `curl /health`.

Image sizes: `square_hd` 1024×1024, `portrait_4_3` 896×1152, `portrait_16_9` 768×1344, `landscape_4_3` 1152×896, `landscape_16_9` 1344×768, or custom 512–2048 (auto-scaled to `MAX_IMAGE_SIDE`).

## Sample workflow: Mame & Mug Shiba Inu Cafe deck

`studio/dist/shiba-cafe-deck/index.html` — 7-slide 16:9 HTML deck (arrows/click/dots/Home/End, `P` print). Tokens: paper `#FAF4E8`, wood `#8B5E34`, accent shiba-orange `#E07A3F`, ink `#231A11`, Baloo 2 + Inter.

| Asset | Prompt head | Size | Seed |
|---|---|---|---|
| `assets/hero.png` | cozy kissaten interior, 2 shibas, noren light, no text | landscape_16_9 1344×768 | 12 |
| `assets/shiba-cream.png` | cream shiba portrait, cream bg, no text | square_hd 1024×1024 | 21 |
| `assets/shiba-red.png` | red shiba portrait, no text | square_hd 1024×1024 | 22 |
| `assets/shiba-blacktan.png` | black-tan shiba portrait, no text | square_hd 1024×1024 | 23 |
| `assets/menu-latte.png` | paw latte art + dorayaki overhead, no text | landscape_4_3 1152×896 | 34 |

Icons (lucide line, `stroke=currentColor`): `lucide-paw-print, lucide-coffee, lucide-dog, lucide-bone, lucide-pin, lucide-clock, lucide-heart, lucide-camera` — 8 IDs, 27 instances. Full token/contract log: `studio/dist/shiba-cafe-deck/DESIGN.md`.

Replay: `POST /submit` → poll `/status/{id}` → `GET /result/{id}` → download `/output/*.png` → copy to `assets/`. Use `/generate` only for quick sync tests (blocks ~3 min).

---

## Engine reference (from z-image-inference)

### Features

- Async queue API + sync `/generate`, web UI, local `.safetensors` weights, 12GB VRAM-safe (CPU offload + attention/VAE slicing, auto-scale to `MAX_IMAGE_SIDE`).

### Installation

```powershell
.\setup.ps1
# or: .\setup.ps1 -CudaIndex cu128
$env:HF_TOKEN = "hf_..."   # if gated configs needed
.\start.ps1
# or: .\start.ps1 -Port 9000 -ModelsDir D:\models
```

UI: `http://localhost:8100` · LAN: `http://<host-ip>:8100` · Swagger: `http://localhost:8100/docs`. Stop: `Ctrl+C`.

### Configuration (env-overridable)

| Variable | Default | Description |
|----------|---------|-------------|
| `PORT` / `PUBLIC_PORT` | `8100` | Listen + result-URL port |
| `ZIMAGE_LOAD_MODE` | `local` (start.ps1) | `local` = `.safetensors`, `hf` = full download |
| `LOCAL_MODELS_DIR` | `C:\Users\Ken Bai\ComfyUI-Shared\models` | `diffusion_models/`, `text_encoders/`, `vae/` |
| `MAX_IMAGE_SIDE` | `1024` | Longest side cap for 12GB VRAM |
| `OUTPUT_DIR` | `.\output` | Generated images |
| `MAX_QUEUE_SIZE` / `MAX_RETRIES` / `JOB_TTL_SECONDS` | `100` / `2` / `3600` | Queue behavior |

Model files (~19.8GB): `diffusion_models/z_image_turbo_bf16.safetensors` (12GB), `text_encoders/qwen_3_4b.safetensors` (7.5GB), `vae/ae.safetensors` (320MB) — source: [Tongyi-MAI/Z-Image-Turbo](https://huggingface.co/Tongyi-MAI/Z-Image-Turbo).

### API

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/` | Web UI |
| `GET` | `/health` | `{status: ok}` / 503 while loading |
| `POST` | `/generate` | Sync generation, inline result |
| `POST` | `/submit` | Async submit → `request_id` |
| `GET` | `/status/{id}` | `IN_QUEUE/IN_PROGRESS/COMPLETED/FAILED` |
| `GET` | `/result/{id}` | Result when COMPLETED |
| `POST` | `/cancel/{id}` | Cancel queued job |
| `GET` | `/api/history` | Recent outputs (20) |
| `GET` | `/output/{file}` | Static image |

Submit fields: `prompt*`, `image_size` (6 presets or custom 512–2048), `num_inference_steps` 1–10 (def 9), `guidance_scale` 0–20 (def 0), `seed`, `num_images` 1–4, `output_format` png|jpeg, `sync_mode`.

### Troubleshooting

- `running scripts is disabled` → `powershell -ExecutionPolicy Bypass -File .\setup.ps1`
- `No .venv found` → run `setup.ps1` first
- `cu130` torch fail → `setup.ps1 -CudaIndex cu128`
- OOM at 1024 → lower `MAX_IMAGE_SIDE=896`, steps=4, `num_images=1`
- `CUDA unavailable` → update driver, check `nvidia-smi`
- 503 health → check start.ps1 terminal (models path, HF_TOKEN, OOM)

## License

Server wrapper; Z-Image-Turbo © Tongyi-MAI/Alibaba — see [model card](https://huggingface.co/Tongyi-MAI/Z-Image-Turbo).
