# Larry — TikTok Marketing Skill (Gemini fork)

Autonomous TikTok slideshow marketing pipeline. Researches competitors, generates AI images, adds text overlays, posts via Postiz, and tracks analytics in a daily feedback loop.

**Original skill:** [clawhub.ai/olliewazza/larry](https://clawhub.ai/olliewazza/larry) by [@OllieWazza](https://github.com/OllieWazza)
**Original license:** MIT-0 — free to use, modify, and redistribute. No attribution required.

---

## What this fork adds

**Gemini image generation** — replaces the OpenAI requirement with Google's Gemini 2.0 Flash native image generation. No OpenAI API key needed.

Changes from upstream:
- `scripts/generate-slides.js` — added `gemini` as a fourth provider option
- `config.example.json` — example config showing Gemini setup

All other files are unchanged from the original Larry v1.0.0.

---

## Quick start

### 1. Get a free Gemini API key

Visit [aistudio.google.com/apikey](https://aistudio.google.com/apikey) — free tier, no billing required.

### 2. Install dependencies

```bash
npm install @google/generative-ai
npm install canvas   # for text overlays (may need build tools — see below)
```

**node-canvas system deps:**

| OS | Command |
|---|---|
| macOS | `brew install pkg-config cairo pango libpng jpeg giflib librsvg` |
| Ubuntu/Debian | `sudo apt-get install build-essential libcairo2-dev libpango1.0-dev libjpeg-dev libgif-dev librsvg2-dev` |
| Windows | Prebuilt binaries download automatically |

### 3. Configure

Copy `config.example.json` to your working directory and fill it in:

```json
{
  "app": {
    "name": "YourAppName",
    "description": "What your app does",
    "targetAudience": "Who it's for",
    "appStoreUrl": "https://apps.apple.com/...",
    "category": "productivity"
  },
  "imageGen": {
    "provider": "gemini",
    "model": "gemini-2.0-flash-preview-image-generation",
    "apiKey": "AIza..."
  },
  "postiz": {
    "apiKey": "your-postiz-api-key",
    "baseUrl": "https://api.postiz.com"
  }
}
```

### 4. Generate slides

```bash
node scripts/generate-slides.js \
  --config tiktok-marketing/config.json \
  --output tiktok-marketing/posts/2026-03-25-1200/ \
  --prompts prompts.json
```

---

## Supported image providers

| Provider | `config.imageGen.provider` | Key required |
|---|---|---|
| Gemini 2.0 Flash | `gemini` | Gemini API key (free) |
| OpenAI gpt-image-1.5 | `openai` | OpenAI API key |
| Stability AI | `stability` | Stability AI API key |
| Replicate | `replicate` | Replicate token |
| Pre-made images | `local` | None |

---

## How the Gemini provider works

Gemini 2.0 Flash generates square images. The script:

1. Injects a portrait composition instruction into the prompt so subjects are centred vertically
2. Calls the Gemini image generation API
3. Uses `canvas` (already required for text overlays) to center-crop scale the result to **1024×1536** (TikTok fullscreen portrait)

No extra dependencies beyond `@google/generative-ai`.

---

## Full pipeline

See [SKILL.md](SKILL.md) for the complete workflow:
- Competitor research
- Slide generation (this file)
- Text overlays (`scripts/add-text-overlay.js`)
- Post to TikTok via Postiz (`scripts/post-to-tiktok.js`)
- Analytics + daily report (`scripts/check-analytics.js`, `scripts/daily-report.js`)

---

## License

MIT-0 — same as upstream. Free to use, modify, and redistribute. No attribution required.
