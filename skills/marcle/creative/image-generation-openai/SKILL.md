---
name: image-generation-openai
description: Use when the user says 生图, 画图, 生成图片, 改图, 图生图, 做海报, 做视觉图, 生成概念图, or similar image-generation/editing language. Calls the local OpenAI-HK image API wrapper script for text-to-image and image-to-image generation. Default for all raster image tasks unless the user explicitly requests SVG or the built-in provider.
version: 1.2.0
author: Hermes Agent
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [image-generation, openai-hk, gpt-image-2, creative]
    related_skills: []
---

# OpenAI-HK Image Generation

## Overview

This skill provides text-to-image and image-to-image generation via the OpenAI-HK API (`gpt-image-2` by default). It is the default image generation path for this user — use it for all raster image tasks (concept art, posters, product images, vehicle renders, etc.) unless the user explicitly asks for SVG output or the built-in provider.

The wrapper script lives alongside this skill at `scripts/openai-hk_image_gen.py`. It handles API key loading, prompt submission, image download, and local saving.

## When to Use

Use this skill when the user says any of:
- 生图, 画图, 生成图片, 改图, 图生图, 做海报, 做视觉图, 生成概念图
- Or any similar image-generation / image-editing request in Chinese or English

Do NOT use this skill when:
- The user explicitly asks for SVG images (use browser-based SVG generation instead)
- The user explicitly asks to use the current provider's built-in image generation

## Requirements

The API key must be stored as `openai-hk_image_key` in the target Hermes profile's `.env` file.

The script checks these locations in order:
- `~/AppData/Local/hermes/.env` (Windows default Hermes profile)
- `~/.hermes/.env` (Unix default Hermes profile)

For named profiles, set the same key in that profile's `.env`, for example:
- `~/AppData/Local/hermes/profiles/<profile>/.env`
- `~/.hermes/profiles/<profile>/.env`

Never print the API key back to the user.

## Install-Time `.env` Setup

When installing this skill, Hermes should also check the target profile's `.env` file and prepare the required key entry.

Use Marcle's standard custom skill/plugin section:

```env
# =============================================================================
# MARCLE SKILL PLUGIN
# =============================================================================
# API key used by skills/marcle/creative/image-generation-openai
openai-hk_image_key=
```

Install behavior:

1. Locate the target profile `.env` file.
   - Default profile: `~/AppData/Local/hermes/.env` on Windows, or `~/.hermes/.env` on Unix.
   - Named profile: `~/AppData/Local/hermes/profiles/<profile>/.env` on Windows, or `~/.hermes/profiles/<profile>/.env` on Unix.
2. Check whether the file already contains the section header `MARCLE SKILL PLUGIN`.
3. If the section does not exist, append the full section shown above near the end of `.env`.
4. If the section already exists but `openai-hk_image_key=` is missing, add these two lines under that section:
   - `# API key used by skills/marcle/creative/image-generation-openai`
   - `openai-hk_image_key=`
5. If `openai-hk_image_key=` already exists, do not duplicate it.
6. Do not fill in or print the actual key unless the user explicitly provides it for this profile.
7. Because `.env` is profile configuration and may contain secrets, ask for explicit confirmation before writing changes.
8. If a gateway for that profile is already running, remind the user to restart it after updating `.env`.

## Usage

All commands are run from the skill directory or with the full path to the script.

### Text-to-Image

```bash
python scripts/openai-hk_image_gen.py "a futuristic electric sedan concept, silver body, studio lighting, automotive design sketch"
```

With full parameters:

```bash
python scripts/openai-hk_image_gen.py "PROMPT" --model gpt-image-2 --size 1024x1024 --quality medium
```

### Ratio + K Auto Sizing

If the user specifies a ratio and resolution tier, use `--ratio` and `--k`:

```bash
python scripts/openai-hk_image_gen.py "汽车概念图" --ratio 16:9 --k 2K
```

The script auto-maps to `2048x1152`. Supported ratio/K combinations:

| Ratio | 1K | 2K | 4K |
|-------|-----|------|------|
| 1:1 | 1024×1024 | 2048×2048 | 2880×2880 |
| 16:9 | 1280×720 | 2048×1152 | 3840×2160 |
| 9:16 | 720×1280 | 1152×2048 | 2160×3840 |
| 5:4 | 1040×832 | 2080×1664 | 3200×2560 |
| 4:5 | 832×1040 | 1664×2080 | 2560×3200 |
| 4:3 | 1024×768 | 2048×1536 | 3264×2448 |
| 3:4 | 768×1024 | 1536×2048 | 2448×3264 |
| 3:2 | 1008×672 | 2064×1376 | 3504×2336 |
| 2:3 | 672×1008 | 1376×2064 | 2336×3504 |
| 21:9 | 1344×576 | 2016×864 | 3808×1632 |

Named aliases are also supported: `square`, `widescreen`, `story`, `print`, `feed`, `classic`, `vertical`, `photo`, `portrait`, `exclusive`.

### Image Edit / Image-to-Image

For one or more reference images, repeat `--image`:

```bash
python scripts/openai-hk_image_gen.py "Generate a photorealistic product poster using the reference images" --image /path/ref1.png --image /path/ref2.png
```

This calls `/v1/images/edits` and sends files as `image[]` form fields.

### Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `prompt` | (required) | Image generation prompt |
| `--model` | `gpt-image-2` | Model: `gpt-image-2`, `gpt-image-1`, `gpt-image-1.5` |
| `--size` | `1024x1024` | Image size (WxH), or `auto` with ratio/K |
| `--ratio` | — | Aspect ratio for auto size lookup |
| `--k` | — | Resolution tier: `1K`, `2K`, `4K` |
| `--quality` | `medium` | Quality: `low`, `medium`, `high`, `auto` |
| `--n` | `1` | Number of images to generate |
| `--response-format` | `url` | API response format: `url` or `b64_json` |
| `--output` | `~/Pictures/hermes-generated/` | Output directory |
| `--image` | — | Reference image path (repeatable, for edits) |
| `--no-download-url` | — | Skip downloading returned URLs; print only |

## Response Handling

The script prints JSON on success:

```json
{
  "success": true,
  "mode": "generation",
  "model": "gpt-image-2",
  "size": "2048x1152",
  "quality": "medium",
  "paths": ["~/Pictures/hermes-generated/openai-hk_gpt-image-2_20250622_143052_1.png"],
  "urls": ["https://...png"],
  "usage": {}
}
```

After successful generation, tell the user the saved image path. If the active platform supports file attachments, attach the generated image.

## Failure Handling

- If the script reports `openai-hk_image_key is not set`, ask the user to add the key to their Hermes `.env` file.
- If the API returns an HTTP error, report the error directly — never invent an image result.

## Common Pitfalls

1. **Running from wrong directory.** The script path is relative to this skill directory. Always `cd` to the skill directory first, or use the full path.
2. **Missing API key.** The key must be in the Hermes `.env` file, not in a shell-exported variable (unless already set in the environment).
3. **Ratio/K mismatch.** Both `--ratio` and `--k` must be provided together when using auto sizing. The combination must be in the supported mappings table.
4. **Reference images not found.** When using `--image`, ensure the paths are absolute or relative to the current working directory.

## Verification Checklist

- [ ] API key is set in `~/.hermes/.env` or `~/AppData/Local/hermes/.env`
- [ ] Python 3 is available
- [ ] Script runs without import errors (stdlib-only, no pip dependencies)
- [ ] Output directory `~/Pictures/hermes-generated/` is writable
