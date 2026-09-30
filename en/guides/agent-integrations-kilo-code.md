---
title: "Integrate Kilo Code"
description: "Use Kilo Code OpenAI-compatible or custom provider settings with Nusabase API."
---

Kilo Code can connect external models through interactive provider configuration. Nusabase provides an OpenAI-compatible Chat Completions endpoint, so use an `OpenAI Compatible` or `Custom OpenAI` provider type.

## When To Use This

- You want to use Nusabase models in Kilo Code.
- You want to switch providers from the model picker.
- You need an OpenAI-compatible API path without changing request format.

## Prerequisites

- Kilo Code CLI or editor extension is installed according to the official Kilo Code docs.
- You have a Nusabase API key: `sk-...`.
- Your machine can reach `https://api.nusabase.io`.
- The example model `qwen3.5-flash` appears in the current pricing docs. Use `/v1/models` as the source of truth.

## Start Kilo Code

Start Kilo Code from your project directory:

```bash
kilo
```

## Connect Nusabase Provider

Open the provider setup flow in Kilo Code:

```text
/connect
```

Choose `OpenAI Compatible`, `Custom OpenAI`, or another provider type that allows a custom Base URL, then enter:

| Setting | Recommended Value |
| --- | --- |
| Provider Name | `Nusabase` |
| Provider Type | `OpenAI Compatible` |
| Base URL | `https://api.nusabase.io/v1` |
| API Key | `sk-your_api_key` |
| Default Model | `qwen3.5-flash` |

<Warning>
Do not choose a fixed DeepSeek provider unless it lets you edit the Base URL. If the Base URL cannot be changed, requests will not go through Nusabase.
</Warning>

## Select Model

Open the model picker:

```text
/models
```

Select the Nusabase model, for example:

```text
Nusabase / qwen3.5-flash
```

To choose another model, query:

```bash
curl https://api.nusabase.io/v1/models \
  -H "Authorization: Bearer sk-your_api_key"
```

## Verify Endpoint

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

## Troubleshooting

| Problem | Likely Cause | Fix |
| --- | --- | --- |
| Nusabase model does not appear | Provider was not saved or model list was not refreshed | Run `/connect` again, then open `/models`. |
| `401` | API key is wrong or has extra spaces | Paste `sk-...` again, then verify with curl. |
| `404` | Base URL or model name is wrong | Use a `/v1` Base URL and a model returned by `/v1/models`. |
| Requests go to DeepSeek directly | A fixed DeepSeek provider was selected | Use OpenAI Compatible / Custom Provider. |

Next: [Error Codes](/en/getting-started/error-codes).

## References

- [Kilo CLI docs](https://kilo.ai/docs/cli)
- [Kilo Code getting started](https://kilo.ai/docs/getting-started)
- [First Request Example](/en/getting-started/first-request)
- [Models and Pricing](/en/getting-started/pricing)
- [Error Codes](/en/getting-started/error-codes)
