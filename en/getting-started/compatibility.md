---
title: "Compatibility Formats"
description: "Use the real Claude, Gemini, and provider-specific compatibility endpoints."
---

Besides the unified `/v1/*` surface, the project keeps a set of real compatibility endpoints.

## Claude compatibility

Endpoint:

- `POST /v1/messages`

Compatible headers:

```http
x-api-key: sk-your_api_key
anthropic-version: 2023-06-01
```

The current model-list endpoint also explicitly supports Anthropic-shaped results when `x-api-key` and `anthropic-version` are present.

## Gemini compatibility

Endpoints:

- `GET /v1beta/models`
- `POST /v1beta/models/{model}:generateContent`
- `POST /v1/engines/{model}/embeddings`

Compatible header:

```http
x-goog-api-key: sk-your_api_key
```

Some Gemini-style paths also support the query-string key form:

```http
GET /v1beta/models?key=sk-your_api_key
```

Use standard `Authorization: Bearer ...` for `GET /v1/models`; do not treat `GET /v1/models?key=...` as the general model-list authentication pattern.

## OpenAI chat prompt extras

`POST /v1/chat/completions` also accepts two optional boolean parameters:

- `get_time`
- `get_weather`

When `get_time=true`, the server injects the current time into the system prompt.
When `get_weather=true`, the server tries to parse a city from the latest user message, fetches a weather summary from Amap, and injects it into the system prompt.

Weather lookup requires this config:

```yaml
weather:
  amap_key: "your Amap Web Service key"
```

## Image and video compatibility endpoints

The current real compatibility set also includes:

- `POST /v1/edits`
- `POST /v1/images/edits`
- `POST /kling/v1/videos/text2video`
- `POST /kling/v1/videos/image2video`
- `POST /jimeng`

These are best when you already have an existing provider-specific request format to preserve.

## Which one to prefer

- New integrations: start with `Core API`
- Existing Claude or Gemini SDKs: use `Compatibility API`
