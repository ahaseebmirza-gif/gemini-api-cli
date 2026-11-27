---
name: gemini-media
description: Use Google Gemini (via the gemini-api CLI) for work Claude can't do natively — generating or editing images (magazine covers, illustrations, hero art), voiceovers (TTS), music, video, transcription, and second-opinion analysis of PDFs/images. Use for "make an image", "magazine cover", "illustration for the article", "voiceover", "transcribe", "ask Gemini".
---

# Gemini media for AHM's magazine work

`gemini-api` is on PATH in the AHM cloud environment (built by the setup script
whenever this repo is attached). Auth is handled by the environment's network
secret: the proxy injects the real key. `GEMINI_API_KEY` is only a placeholder —
never ask for or print the real key.

## Before you start
1. `gemini-api version` — if missing, build it:
   `cd <repo dir> && go build -o /usr/local/bin/gemini-api ./cmd/gemini-api`
   (if that path isn't writable, use the scratchpad and call it by full path).
2. If unsure of flags: `gemini-api <command> --help`, `--dry-run` to preview.

## Common commands
```bash
# Image (default model gemini-nano-banana-2.1); prints written path on stdout
gemini-api image "<detailed prompt>" --out ./magazine/assets/
# Edit / iterate on an image
gemini-api image "Make the masthead area plainer, warmer light" -i ./magazine/assets/cover.jpg --out ./magazine/assets/cover-v2.jpg
# Voiceover
gemini-api tts "Script text" --out ./magazine/audio/intro.wav
# Music, video
gemini-api music "Calm Arabic oud ambient, 30s" --out ./magazine/audio/
gemini-api video "Slow pan over Riyadh skyline at dusk" --out ./magazine/video/
# Analyse a PDF or image
gemini-api analyze -i draft-layout.pdf "Critique layout, typography and hierarchy"
gemini-api transcribe -i interview.m4a --format md --out interview.md
```

## Working rules
- Write detailed prompts: subject, composition, aspect ratio, light, style,
  where text/masthead space should sit. Generate 2–3 variants when choosing a cover.
- Look at each generated image (Read the file) before presenting it; iterate with `-i`.
- Save assets in the project the user is working in; send them to the user with
  SendUserFile. Media is not committed unless the user asks.
- Generated text in images can be misspelt — check it, or ask Gemini for text-free
  art and set type separately.
- Don't generate real people's likeness, logos or trademarks of real brands, or
  imitations of existing publications' covers.
