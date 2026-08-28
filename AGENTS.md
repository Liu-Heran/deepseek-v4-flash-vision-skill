# AGENTS.md

## Purpose

This skill provides a lightweight vision helper for agents without native image input. When an image cannot be read directly, use `scripts/vision.js` to call DeepSeek V4 Flash Vision (`deepseek-v4-flash-vision-exp`) and convert the image into text.

## When to use

- Image path: `node "<SKILL_DIR>/scripts/vision.js" "<absolute image path>" "<prompt>"`
- Image URL: `node "<SKILL_DIR>/scripts/vision.js" --url "<image url>" "<prompt>"`
- Pasted image with no accessible path/URL: `node "<SKILL_DIR>/scripts/vision.js" --clipboard "<prompt>"`

Replace `<SKILL_DIR>` with the actual skill directory. On this machine it is `C:\Users\qq194\.codex\skills\deepseek-v4-vision`.

Fallback rules:

- If a local path does not exist, or no path/URL is provided, `vision.js` automatically tries the system clipboard.
- Pass `--no-fallback` to disable automatic clipboard fallback and fail with an explicit error.
- `--clipboard` cannot be combined with a path or `--url`.

## Configuration

Credentials come from environment variables or a `.env` file next to `vision.js`:

```env
DEEPSEEK_API_KEY=sk-你的Key
DEEPSEEK_BASE_URL=https://api.deepseek.com/v1
VISION_MODEL=deepseek-v4-flash-vision-exp
```

Never commit `.env` or print the API key.

## Rules

- Always use an absolute or explicitly resolved path to `vision.js`.
- Process multiple images one by one and return after all descriptions are ready.
- Use Chinese for descriptions unless the user asks otherwise.
- If the API call fails, report the error and ask the user to check the key, model, or base URL.
