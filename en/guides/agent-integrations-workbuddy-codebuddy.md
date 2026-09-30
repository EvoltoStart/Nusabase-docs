---
title: "Integrate WorkBuddy/CodeBuddy"
description: "Configure WorkBuddy/CodeBuddy with a Nusabase OpenAI-compatible model through models.json."
---

WorkBuddy/CodeBuddy can add custom models through a local `models.json` file. Nusabase Chat Completions can be used as an OpenAI-compatible model endpoint.

## When To Use This

- You want a desktop or editor-based agent coding assistant.
- You need `models.json` to pin team model choices.
- You want to configure lite or reasoning model relationships for CodeBuddy.

## Prerequisites

- WorkBuddy/CodeBuddy is installed and signed in.
- You have a Nusabase API key: `sk-...`.
- Your machine can reach `https://api.nusabase.io`.
- The example model `qwen3.5-flash` appears in the current pricing docs. Use `/v1/models` as the source of truth.

## Config File Location

User-level config:

```text
~/.codebuddy/models.json
```

On Windows this is usually:

```text
C:\Users\<your-user>\.codebuddy\models.json
```

Project-level config:

```text
<project-root>\.codebuddy\models.json
```

Project-level `availableModels` overrides the user-level list instead of merging with it. Do not commit project configs that contain a real API key.

## Write models.json

The CodeBuddy official example expects `apiKey` to contain the actual key value, not an environment variable name.

```json
{
  "models": [
    {
      "id": "nusabase-qwen3.5-flash",
      "name": "Nusabase qwen3.5-flash",
      "vendor": "Nusabase",
      "url": "https://api.nusabase.io/v1/chat/completions",
      "apiKey": "sk-your_api_key",
      "maxInputTokens": 128000,
      "maxOutputTokens": 8192,
      "supportsToolCall": true,
      "supportsImages": false,
      "relatedModels": {
        "lite": "nusabase-qwen3.5-flash",
        "reasoning": "nusabase-qwen3.5-flash"
      }
    }
  ],
  "availableModels": [
    "nusabase-qwen3.5-flash"
  ]
}
```

Field notes:

| Field | Description |
| --- | --- |
| `id` | Internal CodeBuddy model ID. Use an `nusabase-` prefix. |
| `name` | Display name in the model picker. |
| `vendor` | Display vendor, use `Nusabase`. |
| `url` | Full Chat Completions URL, ending in `/v1/chat/completions`. |
| `apiKey` | Actual API key value. |
| `supportsToolCall` | Set to `true` only when the selected model/channel supports tool calls. |
| `supportsImages` | Usually `false` for text models; configure according to real model capability. |

## Restart And Select Model

1. Quit WorkBuddy/CodeBuddy completely.
2. Reopen the app so it reads the new `models.json`.
3. Select `Nusabase qwen3.5-flash` in the model picker.

## Verify API

```bash
curl https://api.nusabase.io/v1/chat/completions \
  -H "Authorization: Bearer sk-your_api_key" \
  -H "Content-Type: application/json" \
  -d '{"model":"qwen3.5-flash","messages":[{"role":"user","content":"hi"}],"stream":false}'
```

## Troubleshooting

| Problem | Likely Cause | Fix |
| --- | --- | --- |
| Model picker does not show the model | Wrong file location or app was not fully restarted | Confirm `.codebuddy/models.json`, then quit and reopen the app. |
| `401` | `apiKey` is wrong or expired | Verify the same key with curl first. |
| `404` | `url` is not the full Chat Completions URL | Use `https://api.nusabase.io/v1/chat/completions`. |
| Project config overrides user config | Project-level `availableModels` does not merge with user-level config | List all required model IDs in project config. |

Use [Models and Pricing](/en/getting-started/pricing) as the source of truth for model names, context length, and price.

## References

- [CodeBuddy models.json guide](https://www.codebuddy.cn/docs/ide/Features/models)
- [CodeBuddy CLI Models](https://www.codebuddy.ai/docs/cli/models)
- [First Request Example](/en/getting-started/first-request)
- [Models and Pricing](/en/getting-started/pricing)
- [Error Codes](/en/getting-started/error-codes)
