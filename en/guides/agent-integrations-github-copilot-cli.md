---
title: "Integrate GitHub Copilot CLI"
description: "Use Copilot CLI BYOK environment variables with the Nusabase OpenAI-compatible endpoint."
---

GitHub Copilot CLI supports BYOK (Bring Your Own Key) for OpenAI Chat Completions-compatible endpoints. The Nusabase OpenAI-compatible Base URL is `https://api.nusabase.io/v1`.

## When To Use This

- You want to use Copilot CLI in the terminal.
- You want Copilot CLI model calls to go through Nusabase.
- Your selected model supports tool calling and streaming.

<Warning>
Copilot CLI BYOK requires the model to support tool calling/function calling and streaming. If the selected model or channel does not support these capabilities, Copilot CLI will return an error. Confirm available models with `/v1/models`, then choose according to model capability.
</Warning>

## Prerequisites

- GitHub Copilot CLI is installed according to GitHub's official docs.
- You have a Nusabase API key: `sk-...`.
- Your machine can reach `https://api.nusabase.io`.
- The example model `qwen3.5-flash` appears in the current pricing docs. Use `/v1/models` as the source of truth.

Check the CLI:

```bash
copilot --version
```

## Configure Environment Variables

Linux / macOS / WSL:

```bash
export COPILOT_PROVIDER_TYPE=openai
export COPILOT_PROVIDER_BASE_URL=https://api.nusabase.io/v1
export COPILOT_PROVIDER_API_KEY=sk-your_api_key
export COPILOT_MODEL=qwen3.5-flash
```

Windows PowerShell:

```powershell
$env:COPILOT_PROVIDER_TYPE="openai"
$env:COPILOT_PROVIDER_BASE_URL="https://api.nusabase.io/v1"
$env:COPILOT_PROVIDER_API_KEY="sk-your_api_key"
$env:COPILOT_MODEL="qwen3.5-flash"
```

| Environment Variable | Description |
| --- | --- |
| `COPILOT_PROVIDER_TYPE` | Use `openai` for OpenAI Chat Completions-compatible calls. |
| `COPILOT_PROVIDER_BASE_URL` | Use `https://api.nusabase.io/v1`. |
| `COPILOT_PROVIDER_API_KEY` | Nusabase API key. |
| `COPILOT_MODEL` | Default model name; must be enabled for your account. |

If the model is not in Copilot CLI's built-in catalog, set token limits explicitly:

```bash
export COPILOT_PROVIDER_MAX_PROMPT_TOKENS=128000
export COPILOT_PROVIDER_MAX_OUTPUT_TOKENS=8192
```

## Verify API First

```bash
curl https://api.nusabase.io/v1/chat/completions \
  -H "Authorization: Bearer sk-your_api_key" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen3.5-flash",
    "messages": [{"role": "user", "content": "hello"}],
    "stream": true
  }'
```

## Start Copilot CLI

```bash
copilot
```

Then send:

```text
Introduce this project in one sentence.
```

## Troubleshooting

| Problem | Likely Cause | Fix |
| --- | --- | --- |
| `401` | API key is wrong or not visible in the current shell | Set `COPILOT_PROVIDER_API_KEY` again in the same terminal. |
| `404` or model unavailable | `COPILOT_MODEL` is not enabled for your account | Call `/v1/models` and copy a real model ID. |
| `400` | Provider type or Base URL does not match the CLI request format | Use `openai` and `https://api.nusabase.io/v1`. |
| Tool calls fail | The selected model or channel does not support tool calling/streaming | Switch to a model that supports tool calling and streaming. |
| CLI still calls GitHub-hosted models | Environment variables were not inherited | Set them in the same terminal that starts `copilot`. |

For VS Code workflows, see [GitHub Copilot](/en/guides/agent-integrations-github-copilot).

## References

- [GitHub Copilot CLI BYOK](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/use-byok-models)
- [GitHub Copilot CLI product page](https://github.com/features/copilot/cli)
- [First Request Example](/en/getting-started/first-request)
- [Models and Pricing](/en/getting-started/pricing)
- [Error Codes](/en/getting-started/error-codes)
