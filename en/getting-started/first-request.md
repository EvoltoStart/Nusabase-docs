---
title: "First Request Example"
description: "Run a minimal working call from model listing to your first Chat Completions request."
---

This page shows the shortest practical call flow: verify your API key first, then send your first model request. The examples use the standard OpenAI-compatible entry point, which is the recommended starting point for new integrations, backend services, and command-line debugging.

Before you start, prepare:

| Item | Example | Notes |
| --- | --- | --- |
| API Base URL | `https://api.nusabase.io` | The English docs use this domain in examples. |
| API Key | `sk-your_api_key` | Send it as a Bearer token in the request header. |
| Model | `qwen3.5-flash` | This example model appears in the current pricing docs. Use `GET /v1/models` as the source of truth for your account. |

If you are not sure which header to use, read [Authentication](/en/getting-started/authentication) first.

## 1. Set Environment Variables

Put the base URL and API key into environment variables so the following examples can be copied directly.

```bash
export NUSABASE_BASE_URL="https://api.nusabase.io"
export NUSABASE_API_KEY="sk-your_api_key"
```

Windows PowerShell:

```powershell
$env:NUSABASE_BASE_URL = "https://api.nusabase.io"
$env:NUSABASE_API_KEY = "sk-your_api_key"
```

## 2. List Models

Start with `GET /v1/models`. This does not generate model output, so it is the safest first call for checking your API key, account status, and available models.

```bash
curl "$NUSABASE_BASE_URL/v1/models" \
  -H "Authorization: Bearer $NUSABASE_API_KEY"
```

If the request succeeds, the response contains the models your account can access. Choose the `model` value in later requests from this response.

You can also retrieve one model:

```bash
curl "$NUSABASE_BASE_URL/v1/models/qwen3.5-flash" \
  -H "Authorization: Bearer $NUSABASE_API_KEY"
```

If this step fails, start with these checks:

| Status | Common cause | What to check |
| --- | --- | --- |
| `401` | Missing, invalid, expired, or exhausted API key | Check `Authorization: Bearer ...` and make sure you are using an API key. |
| `403` | Account, group, IP allowlist, or API access permission is not valid for the request | Check account status, key permissions, group access, and IP restrictions. |
| `503` | The selected model or channel is temporarily unavailable | Retry later or use another model returned by the model list. |

## 3. Send Your First Chat Request

Use `POST /v1/chat/completions` for the first inference request. It follows the OpenAI Chat Completions shape and has a small, easy-to-debug request body.

```bash
curl "$NUSABASE_BASE_URL/v1/chat/completions" \
  -H "Authorization: Bearer $NUSABASE_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen3.5-flash",
    "messages": [
      {
        "role": "system",
        "content": "You are a concise and accurate assistant."
      },
      {
        "role": "user",
        "content": "Introduce Nusabase API in one sentence."
      }
    ]
  }'
```

Request body fields:

| Field | Required | Description |
| --- | --- | --- |
| `model` | Yes | The model to call. Prefer a model returned by `GET /v1/models`. |
| `messages` | Yes | Conversation message array, usually with `system`, `user`, and `assistant` roles. |
| `role` | Yes | Message role. The first user input should use `user`. |
| `content` | Yes | Message content. Text models can use a plain string. |

A successful response usually contains `choices`; the model output is in `choices[0].message.content`.

```json
{
  "choices": [
    {
      "message": {
        "role": "assistant",
        "content": "Nusabase API is an aggregation platform for calling multiple AI models through compatible APIs."
      }
    }
  ]
}
```

<Note>
The sample response only shows the key fields. Real responses may also include `id`, `object`, `created`, `model`, `usage`, and other fields.
</Note>

## 4. Enable Streaming

Set `stream` to `true` if you want to receive tokens as they are generated.

```bash
curl "$NUSABASE_BASE_URL/v1/chat/completions" \
  -H "Authorization: Bearer $NUSABASE_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen3.5-flash",
    "stream": true,
    "messages": [
      {
        "role": "user",
        "content": "Give me a three-point summary."
      }
    ]
  }'
```

Streaming is useful for chat UIs, long generation tasks, and any workflow that should show the first output quickly. Your client should read the response incrementally using Server-Sent Events or a compatible streaming parser.

## 5. Responses Example

If your client already targets the OpenAI Responses style, call `POST /v1/responses`.

```bash
curl "$NUSABASE_BASE_URL/v1/responses" \
  -H "Authorization: Bearer $NUSABASE_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen3.5-flash",
    "input": "Introduce Nusabase API in one sentence."
  }'
```

The project also exposes a context-compaction endpoint:

```bash
curl "$NUSABASE_BASE_URL/v1/responses/compact" \
  -H "Authorization: Bearer $NUSABASE_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen3.5-flash",
    "input": "Compact this conversation context."
  }'
```

Whether a model supports Responses or compaction depends on the available backend channel configuration. If the request returns a model or channel availability error, switch to a model returned by `GET /v1/models`.

## 6. Compatibility Examples

If you already have a Claude or Gemini SDK, you can use the compatible entry points for a first smoke test.

Claude Messages example:

```bash
curl "$NUSABASE_BASE_URL/v1/messages" \
  -H "x-api-key: $NUSABASE_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen3.5-flash",
    "max_tokens": 128,
    "messages": [
      {
        "role": "user",
        "content": "hello"
      }
    ]
  }'
```

Gemini model-list example:

```bash
curl "$NUSABASE_BASE_URL/v1beta/models" \
  -H "x-goog-api-key: $NUSABASE_API_KEY"
```

You can also use the query-string key form:

```bash
curl "$NUSABASE_BASE_URL/v1beta/models?key=$NUSABASE_API_KEY"
```

Compatibility entry points may use different authentication headers and response shapes from the standard `/v1/*` API. See [Compatibility Formats](/en/getting-started/compatibility) for details.

## 7. Next Steps

After your first request succeeds, continue with:

| Doc | Use it for |
| --- | --- |
| [Authentication](/en/getting-started/authentication) | API key, Bearer auth, Claude, Gemini, and WebSocket authentication. |
| [Models and Pricing](/en/getting-started/pricing) | Choosing a model and checking input, output, cache, and other prices. |
| [Billing Rules](/en/getting-started/billing-rules) | Understanding usage-based, per-call, duration-based, resolution-based, and tiered billing. |
| [Error Codes](/en/getting-started/error-codes) | Debugging failures by `HTTP status` and `error.code`. |

## References

- [Authentication](/en/getting-started/authentication)
- [Compatibility Formats](/en/getting-started/compatibility)
- [Models and Pricing](/en/getting-started/pricing)
- [Error Codes](/en/getting-started/error-codes)
