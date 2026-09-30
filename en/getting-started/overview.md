---
title: "Quick Start"
description: "Recommended integration path from preparing an API key to making the first model call."
---

If this is your first model integration, follow this page to set up authentication, list models, and send your first request.

## Integration Path

Start with these sections:

1. `Core API`
    - The unified inference surface.
    - Includes `/v1/models`, `/v1/chat/completions`, `/v1/responses`, `/v1/embeddings`, `/v1/images/generations`, `/v1/audio/*`, and `/v1/videos*`.

2. `Compatibility API`
    - For Claude, Gemini, Kling, Jimeng, and other provider-specific or compatibility formats.
    - Includes `/v1/messages`, `/v1beta/models/{model}:generateContent`, `/kling/*`, and `/jimeng`.

## Base URLs

- Inference and compatibility APIs: `https://api.nusabase.io`

## Recommended path

1. Prepare your API key (`sk-...`)
2. Call `GET /v1/models`
3. Send your first `POST /v1/chat/completions`
4. If you already use Claude or Gemini SDKs, continue with [Compatibility Formats](/en/getting-started/compatibility)

## Smallest working request

```bash
curl https://api.nusabase.io/v1/chat/completions \
  -H "Authorization: Bearer sk-your_api_key" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-4o-mini",
    "messages": [
      {"role": "user", "content": "hello"}
    ]
  }'
```

Next, read [Authentication](/en/getting-started/authentication), then complete [First Request Example](/en/getting-started/first-request).
