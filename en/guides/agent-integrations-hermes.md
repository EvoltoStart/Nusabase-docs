---
title: "Integrate Hermes"
description: "Configure Hermes Agent model provider with Nusabase API."
---

Hermes Agent can select model providers through `hermes model` or `hermes setup`. Nusabase can be used through either an OpenAI-compatible or Anthropic-compatible setup.

## When To Use This

- You want Hermes long-term memory, skills, and tools.
- You want model requests to go through Nusabase API.
- You need to switch providers or models inside Hermes.

## Prerequisites

- You have a Nusabase API key: `sk-...`.
- Your machine can reach `https://api.nusabase.io`.
- The example model `qwen3.5-flash` appears in the current pricing docs. Use `/v1/models` as the source of truth.

## Install Hermes

Linux / macOS / WSL2:

```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

Windows users can run the same installer from WSL2. Do not use a PowerShell URL unless it is provided by the Hermes Agent project.

Reload your shell, then check:

```bash
hermes --version
```

## Configure Model

Run:

```bash
hermes model
```

Or run full first-time setup:

```bash
hermes setup
```

Use these values in the Hermes UI:

| Setting | Recommended Value |
| --- | --- |
| Provider | Choose `OpenAI Compatible` / `Custom`, or any provider that lets you edit Base URL. |
| OpenAI-compatible Base URL | `https://api.nusabase.io/v1` |
| Anthropic-compatible Base URL | `https://api.nusabase.io` |
| API Key | `sk-your_api_key` |
| Model | `qwen3.5-flash`, or a real model returned by `/v1/models`. |

<Warning>
OpenAI-compatible setup uses the `/v1` Base URL. Anthropic-compatible setup uses the root URL so the tool can append `/v1/messages`. Do not paste another provider's official API URL into Hermes.
</Warning>

## Verify Nusabase

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

## Start Hermes

```bash
hermes
```

Send:

```text
Read the current project structure and give three improvement suggestions.
```

## Troubleshooting

| Problem | Likely Cause | Fix |
| --- | --- | --- |
| Provider does not allow a custom Base URL | A fixed official provider was selected | Use OpenAI Compatible / Custom Provider. |
| `401` | API key is invalid or not saved | Run `hermes model` or `hermes setup` again. |
| `404` | Model name or Base URL is wrong | Query `/v1/models`; for OpenAI-compatible setup, use the `/v1` Base URL. |
| Anthropic mode fails | Base URL includes `/v1` | Use root URL `https://api.nusabase.io` for Anthropic-compatible setup. |

Next, see [First Request Example](/en/getting-started/first-request).

## References

- [Hermes Agent docs](https://hermes-agent.nousresearch.com/docs/)
- [Hermes Agent GitHub](https://github.com/NousResearch/hermes-agent)
- [Hermes Agent quickstart](https://github.com/NousResearch/hermes-agent/blob/main/website/docs/getting-started/quickstart.md)
- [First Request Example](/en/getting-started/first-request)
- [Error Codes](/en/getting-started/error-codes)
