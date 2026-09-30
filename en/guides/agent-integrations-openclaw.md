---
title: "Integrate OpenClaw"
description: "Configure OpenClaw custom model provider with Nusabase API."
---

Use this guide to connect OpenClaw model calls to Nusabase API. The Nusabase OpenAI-compatible Base URL is `https://api.nusabase.io/v1`.

## When To Use This

- You need explicit multi-provider model management in OpenClaw.
- You want to inspect models with `/models` or `openclaw models list`.
- You want the default model to point to a Nusabase gateway model.

## Prerequisites

- OpenClaw is installed.
- You have a Nusabase API key: `sk-...`.
- Your machine can reach `https://api.nusabase.io`.
- The example models `qwen3.5-flash` and `qwen3.5-plus` appear in the current pricing docs. Use `GET /v1/models` as the source of truth for your account.

## Verify Nusabase First

```bash
curl https://api.nusabase.io/v1/models \
  -H "Authorization: Bearer sk-your_api_key"
```

```bash
curl https://api.nusabase.io/v1/chat/completions \
  -H "Authorization: Bearer sk-your_api_key" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen3.5-flash",
    "messages": [{"role": "user", "content": "hello"}],
    "stream": false
  }'
```

## Configure Provider

OpenClaw recommends `--strict-json --merge` when changing provider and model allowlist maps, so existing config is not overwritten accidentally.

```bash
openclaw config set models.providers.nusabase '{
  "api": "openai-completions",
  "baseUrl": "https://api.nusabase.io/v1",
  "apiKey": "sk-your_api_key",
  "models": [
    {"id": "qwen3.5-flash"},
    {"id": "qwen3.5-plus"}
  ]
}' --strict-json --merge
```

If your OpenClaw version does not support writing a JSON block from CLI, edit the OpenClaw config file and keep the same fields:

```json
{
  "models": {
    "providers": {
      "nusabase": {
        "api": "openai-completions",
        "baseUrl": "https://api.nusabase.io/v1",
        "apiKey": "sk-your_api_key",
        "models": [
          {"id": "qwen3.5-flash"},
          {"id": "qwen3.5-plus"}
        ]
      }
    }
  }
}
```

## Set Default Model

```bash
openclaw models set nusabase/qwen3.5-flash
```

If model allowlists are enabled, add the models to `agents.defaults.models`:

```bash
openclaw config set agents.defaults.models '{
  "nusabase/qwen3.5-flash": {},
  "nusabase/qwen3.5-plus": {}
}' --strict-json --merge
```

## Verify

```bash
openclaw models list --provider nusabase
openclaw models status
openclaw run "Please introduce the current project."
```

Success criteria:

- `openclaw models list --provider nusabase` shows Nusabase models.
- `openclaw models status` shows `nusabase/qwen3.5-flash` as the default model.
- `openclaw run` returns a normal response.

## Troubleshooting

| Problem | Likely Cause | Fix |
| --- | --- | --- |
| `Model is not allowed` | Allowlist is enabled but Nusabase model is not listed | Add the model with the `agents.defaults.models` merge command. |
| `401` | API key is wrong | Reset `models.providers.nusabase.apiKey`. |
| `404` | Base URL is wrong | Use `https://api.nusabase.io/v1`, not the root URL. |
| Empty model list | Provider was not saved or model IDs are wrong | Confirm `/v1/models`, then update `models.providers.nusabase.models`. |

Next, see [Error Codes](/en/getting-started/error-codes) for API failure meanings.

## References

- [OpenClaw configuration and custom providers](https://docs.openclaw.ai/gateway/config-tools)
- [OpenClaw Models CLI](https://documentation.openclaw.ai/concepts/models)
- [First Request Example](/en/getting-started/first-request)
- [Models and Pricing](/en/getting-started/pricing)
- [Error Codes](/en/getting-started/error-codes)
