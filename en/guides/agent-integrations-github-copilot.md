---
title: "Integrate GitHub Copilot"
description: "How to use Nusabase API in GitHub Copilot workflows, including limitations, BYOK-style setup, validation, and troubleshooting."
---

This guide is for users who already work in VS Code with GitHub Copilot Chat and want to route part of their model traffic through Nusabase API.

<Note>
The official GitHub Copilot Chat client does not always expose a general field for arbitrary OpenAI-compatible Base URLs. To use Nusabase in a Copilot Chat model picker, use a Copilot extension or BYOK workflow that supports custom providers. For a more predictable terminal setup, see [GitHub Copilot CLI](/en/guides/agent-integrations-github-copilot-cli).
</Note>

## When To Use GitHub Copilot

- You mainly work in VS Code.
- You already use GitHub Copilot.
- You want to keep Copilot Chat, agent mode, tool use, and MCP workflows.
- Your Copilot extension supports custom API keys, Base URLs, and model names.

## Prerequisites

- VS Code installed.
- GitHub Copilot installed and signed in.
- A valid API key: `sk-...`.
- Network access to `https://api.nusabase.io`.
- At least one available model from `GET /v1/models`.

## Validate Nusabase First

Before configuring Copilot, confirm that your API key and model work.

```bash
curl https://api.nusabase.io/v1/models \
  -H "Authorization: Bearer sk-your_api_key"
```

Then test Chat Completions:

```bash
curl https://api.nusabase.io/v1/chat/completions \
  -H "Authorization: Bearer sk-your_api_key" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen3.5-flash",
    "messages": [
      {
        "role": "user",
        "content": "hello"
      }
    ],
    "stream": false
  }'
```

## Configuration Values

If your Copilot extension supports custom models or OpenAI-compatible providers, use these values:

| Setting | Recommended value | Notes |
| --- | --- | --- |
| Provider type | `openai-compatible` or `openai` | The exact name depends on the extension. |
| Base URL | `https://api.nusabase.io/v1` | If the extension requires a full endpoint, use `https://api.nusabase.io/v1/chat/completions`. |
| API Key | `sk-your_api_key` | Do not paste the Base URL into the API key field. |
| Model | `qwen3.5-flash` | Example only. Use `/v1/models` as the source of truth. |
| Streaming | Enabled | Copilot Chat usually benefits from streaming output. |

Common flow:

1. Open the VS Code command palette.
2. Open the extension's Provider, Model, or BYOK configuration screen.
3. Add an OpenAI-compatible provider named `Nusabase`.
4. Fill in Base URL, API key, and model name.
5. Return to Copilot Chat and select the Nusabase model.

## Model Strategy

| Scenario | Recommendation |
| --- | --- |
| Daily Q&A and lightweight code explanations | Use a fast, low-cost model. |
| Multi-file edits and deeper reasoning | Use a stronger model with a larger context window. |
| Team-wide setup | Fix the provider name and default model in workspace or extension settings. |

For actual model availability and pricing, see [Models and Pricing](/en/getting-started/pricing).

## Validation

- The Nusabase model appears in the Copilot model picker.
- A `hello` prompt returns a valid response.
- Errors expose Nusabase or upstream HTTP status information.

## Troubleshooting

| Problem | Likely cause | Fix |
| --- | --- | --- |
| No Base URL field in native Copilot settings | The current client may not support arbitrary custom providers | Use a BYOK/custom-provider extension or switch to Copilot CLI. |
| `401` | Missing or invalid API key | Confirm that `sk-...` is in the API key field. |
| `404` or model unavailable | The model name is not available to your account | Call `/v1/models` and copy the returned model ID. |
| Slow response | Large model, long context, or network latency | Reduce context, use a faster model, and keep streaming enabled. |

Next: [GitHub Copilot CLI](/en/guides/agent-integrations-github-copilot-cli).

## References

- [GitHub Copilot docs](https://docs.github.com/en/copilot)
- [GitHub Copilot CLI BYOK](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/use-byok-models)
- [GitHub Copilot CLI product page](https://github.com/features/copilot/cli)
- [First Request Example](/en/getting-started/first-request)
- [Models and Pricing](/en/getting-started/pricing)
