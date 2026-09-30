---
title: "Integrate Claude Code"
description: "Configure Claude Code through the Nusabase Anthropic Messages-compatible endpoint."
---

Use this guide to route Claude Code model calls through Nusabase. Claude Code appends `/v1/messages` to `ANTHROPIC_BASE_URL`, so configure the root URL: `https://api.nusabase.io`.

## When To Use This

- You primarily use Claude Code from the terminal.
- You want to keep the Anthropic Messages request format.
- You want API keys, quota, and usage to go through Nusabase.

## Prerequisites

- Claude Code is installed.
- You have a Nusabase API key: `sk-...`.
- Your machine can reach `https://api.nusabase.io`.
- The example model `qwen3.5-flash` appears in the current pricing docs. Use `/v1/models` as the source of truth.

## Configure Environment Variables

Linux / macOS / WSL:

```bash
export ANTHROPIC_BASE_URL=https://api.nusabase.io
export ANTHROPIC_AUTH_TOKEN=sk-your_api_key
export ANTHROPIC_MODEL=qwen3.5-flash
```

Windows PowerShell:

```powershell
$env:ANTHROPIC_BASE_URL="https://api.nusabase.io"
$env:ANTHROPIC_AUTH_TOKEN="sk-your_api_key"
$env:ANTHROPIC_MODEL="qwen3.5-flash"
```

Configuration notes:

| Setting | Correct Value | Notes |
| --- | --- | --- |
| `ANTHROPIC_BASE_URL` | `https://api.nusabase.io` | Use the root URL, without `/v1`. |
| `ANTHROPIC_AUTH_TOKEN` | `sk-...` | Claude Code sends this as Authorization Bearer. |
| `ANTHROPIC_API_KEY` | `sk-...` | Optional alternative; used as `x-api-key` when `AUTH_TOKEN` is not set. |
| `ANTHROPIC_MODEL` | `qwen3.5-flash` | Use a real model returned by `/v1/models`. |

## Persistent Configuration

Write user-level config to `~/.claude/settings.json`:

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.nusabase.io",
    "ANTHROPIC_AUTH_TOKEN": "sk-your_api_key",
    "ANTHROPIC_MODEL": "qwen3.5-flash"
  }
}
```

For shared project config, do not include a real API key:

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.nusabase.io",
    "ANTHROPIC_MODEL": "qwen3.5-flash"
  }
}
```

## Verify Nusabase

```bash
curl https://api.nusabase.io/v1/models \
  -H "Authorization: Bearer sk-your_api_key"
```

```bash
curl https://api.nusabase.io/v1/messages \
  -H "x-api-key: sk-your_api_key" \
  -H "anthropic-version: 2023-06-01" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen3.5-flash",
    "max_tokens": 256,
    "messages": [{"role": "user", "content": "hello"}]
  }'
```

## Start Claude Code

```bash
claude
```

In the interactive session, send:

```text
Please summarize the current project structure.
```

## Model Selection Notes

Claude Code gateway model discovery only adds some Anthropic-style model IDs to the model picker. For non-Claude names such as `qwen3.5-flash`, set `ANTHROPIC_MODEL` explicitly.

To try gateway model discovery:

```bash
export CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY=1
```

## Troubleshooting

| Problem | Likely Cause | Fix |
| --- | --- | --- |
| `404` | `ANTHROPIC_BASE_URL` includes `/v1` | Use `https://api.nusabase.io`. |
| `401` | API key is wrong or sent with the wrong header | Prefer `ANTHROPIC_AUTH_TOKEN=sk-...`, then verify with curl. |
| Model unavailable | `ANTHROPIC_MODEL` is not enabled for your account | Call `/v1/models`, then copy an available model name. |
| qwen model not shown in picker | Claude Code gateway discovery filters model IDs | Set `ANTHROPIC_MODEL` explicitly. |

Next, see [First Request Example](/en/getting-started/first-request).

## References

- [Claude Code LLM gateway](https://code.claude.com/docs/en/llm-gateway)
- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Anthropic Messages API](https://docs.anthropic.com/en/api/messages)
- [First Request Example](/en/getting-started/first-request)
- [Error Codes](/en/getting-started/error-codes)
