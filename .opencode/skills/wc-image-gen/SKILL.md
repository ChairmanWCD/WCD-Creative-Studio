---
name: wc-image-gen
description: Generate White Crane images via local Z-Image inference server. Use when brief needs hero images, posters, or visual assets.
---

# WC Image Gen

Server (WCD default): `http://localhost:8100` — docs at `http://localhost:8100/docs`.
LAN override (legacy White Crane Studio box): `http://192.168.1.202:8100`.

Prompt library: `.opencode/vendor/open-design/prompt-templates/image/` (93 prompts with model, ratio, attribution).

Flow:
1. `GET /health` — expect `{status: ok}`.
2. `POST /generate`:
```json
{"prompt": "...", "image_size": "square_hd", "num_images": 1, "output_format": "png", "sync_mode": false}
```
Sizes: square_hd 1024x1024, portrait_4_3 896x1152, portrait_16_9 768x1344, landscape_4_3 1152x896, landscape_16_9 1344x768, or custom {width,height} 512-2048.
3. Queue alternative: `POST /submit` -> `GET /status/{id}` -> `GET /result/{id}`. Cancel via `POST /cancel/{id}`.
4. Save working files under `studio/dist/`. Reference generation outputs by their `/output/*.png` URL. Never commit base64 blobs.

Return image URL, prompt, seed, size, and model (`z_image_turbo` / `flux2_klein_9b`).
