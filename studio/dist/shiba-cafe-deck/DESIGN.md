# Mame & Mug — Shiba Inu Café · DESIGN.md delta

Brand direction: warm cozy modern kissaten + playful Shiba personality.
Extends the White Crane base contract; these tokens are scoped to `studio/dist/shiba-cafe-deck/`.

## Tokens added (v1 · mame-mug-kissaten)

| Token | Value | Usage |
|---|---|---|
| `--paper` | `#FAF4E8` | cream paper slide background |
| `--paper-deep` | `#F2E7D0` | photo wells, placeholder fills |
| `--card` | `#FFFDF7` | cards on paper |
| `--wood` | `#8B5E34` | wood accents, alternating steps |
| `--wood-deep` | `#5C3D21` | page surround, footer depth |
| `--accent` | `#E07A3F` | shiba-orange: CTAs, kickers, highlights |
| `--accent-deep` | `#C05E27` | pressed states, kicker text |
| `--accent-soft` | `#F7D9BE` | icon chips, coat tags |
| `--ink` | `#231A11` | warm ink-black text, dark cards |
| `--muted` | `#7C6B56` | secondary text |
| `--leaf` | `#6F8B5E` | welfare/care accents only |
| `--line` | `#E7D8BC` | hairlines, card borders |
| `--radius` | `22px` | friendly rounded corners |

Typography: **Baloo 2** (rounded friendly display) + **Inter** (clean grotesque body),
Google Fonts with system fallback (`Hiragino Maru Gothic ProN`, `Yu Gothic`, system-ui).
No cold corporate blue. No stock gradients (one warm photo-overlay gradient only, for legibility).

## Icons (all lucide, line style, stroke=currentColor)

`lucide-paw-print`, `lucide-coffee`, `lucide-dog`, `lucide-bone`,
`lucide-pin`, `lucide-clock`, `lucide-heart`, `lucide-camera`.
Recolored to `--ink` / `--accent-deep` / `--paper` via `.iconchip` variants.

## Images (White Crane inference `z_image_turbo`)

| File | Prompt head | Size | Seed | Request |
|---|---|---|---|---|
| `assets/hero.png` | cozy kissaten interior, 2 shibas, noren light | landscape_16_9 (1344×768) | 12 | `5fbabfe2…` |
| `assets/shiba-cream.png` | cream shiba portrait, cream bg | square_hd (1024×1024) | 21 | `c89fe84f…` |
| `assets/shiba-red.png` | red shiba portrait, cream bg | square_hd (1024×1024) | 22 | `3ecd13b9…` |
| `assets/shiba-blacktan.png` | black-tan shiba portrait, cream bg | square_hd (1024×1024) | 23 | `fde776f8…` |
| `assets/menu-latte.png` | paw latte art + dorayaki, overhead | landscape_4_3 (1152×896) | 34 | `40877f26…` |

All prompts suffixed `no text, no watermark`. Originals live on the inference box under `/output/`;
deck references local `assets/*.png` copies (self-contained except local images).

## Contract check

- Cream paper + wood + shiba-orange everywhere; ink stays warm (`#231A11`, never pure black/blue).
- One rounded display (Baloo 2) + one grotesque body (Inter) — no mixing.
- Single accent discipline: orange leads; leaf-green appears only in Care section.
- Print: `@media print` stacks all 7 slides, hides chrome, keeps cards legible.
