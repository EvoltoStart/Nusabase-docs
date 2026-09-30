---
title: "Authentication"
description: "Authentication methods, compatibility headers, and common auth failures for public APIs."
---

All public model calls are validated with an API key. Use standard Bearer authentication by default. Use compatibility headers only when you are integrating an existing Claude or Gemini client.

## Standard Authentication

Standard authentication applies to core `/v1/*` endpoints, including:

- `GET /v1/models`
- `GET /v1/models/{model}`
- `POST /v1/chat/completions`
- `POST /v1/completions`
- `POST /v1/responses`
- `POST /v1/responses/compact`
- `POST /v1/embeddings`
- `POST /v1/images/generations`
- `POST /v1/images/edits`
- `POST /v1/audio/transcriptions`
- `POST /v1/audio/translations`
- `POST /v1/audio/speech`
- `POST /v1/audio/generations`
- `POST /v1/rerank`

Header:

```http
Authorization: Bearer sk-your_api_key
```

The `Bearer` prefix is case-insensitive, but the format above is recommended. API keys usually start with `sk-`.

## Claude-Compatible Authentication

The Claude Messages-compatible endpoint is:

- `POST /v1/messages`

Headers:

```http
x-api-key: sk-your_api_key
anthropic-version: 2023-06-01
```

`GET /v1/models` and `GET /v1/models/{model}` return Anthropic / Claude-compatible model shapes when both `x-api-key` and `anthropic-version` are present.

## Gemini-Compatible Authentication

Gemini-compatible endpoints include:

- `GET /v1beta/models`
- `GET /v1beta/openai/models`
- `POST /v1beta/models/{model}:generateContent`
- `POST /v1/engines/{model}/embeddings`

Recommended header:

```http
x-goog-api-key: sk-your_api_key
```

Some Gemini-style paths also support the query-string key form:

```http
GET /v1beta/models?key=sk-your_api_key
```

Note: use standard `Authorization: Bearer ...` for `GET /v1/models`. Do not treat `GET /v1/models?key=...` as the general model-list authentication pattern.

## WebSocket Authentication

`GET /v1/realtime` is the WebSocket entry point and uses the same API key system. Prefer:

```http
Authorization: Bearer sk-your_api_key
```

If your WebSocket client can only pass credentials through subprotocols, `Sec-WebSocket-Protocol` also supports the `openai-insecure-api-key.{key}` form.

## What Is Checked

The server validates:

- whether the API key exists
- whether the API key is expired, disabled, or out of quota
- whether the user account is enabled and allowed to use API access
- whether the API key has access to the requested group
- whether the request IP is allowed when the key has IP restrictions

## Common Auth Issues

| Status | Common cause | What to check |
| --- | --- | --- |
| `401` | Missing, invalid, malformed, expired, or exhausted API key | Check the auth header and make sure you are using an API key, not a web login token |
| `403` | User disabled, API access disabled, group denied, or IP outside allowlist | Check account status, key permissions, group access, and IP restrictions |
| `429` | Rate limit exceeded | Reduce concurrency or retry later |

Next: [First Request Example](/en/getting-started/first-request).
