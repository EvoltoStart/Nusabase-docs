---
title: "Integration Overview"
description: "Entry page for Claude Code, GitHub Copilot, OpenCode, OpenClaw, and other agent tools with Nusabase API."
---

Use this page as the agent tool integration entry point. Different tools support custom providers, OpenAI-compatible APIs, and Anthropic-compatible APIs in different ways, so follow the dedicated guide for your tool.

## Common Configuration

- Root URL: `https://api.nusabase.io`
- OpenAI-compatible Base URL: `https://api.nusabase.io/v1`
- OpenAI-compatible auth header: `Authorization: Bearer sk-your_api_key`
- Anthropic-compatible auth headers: `x-api-key: sk-your_api_key` and `anthropic-version: 2023-06-01`
- Model discovery: `GET /v1/models`
- Chat Completions: `POST /v1/chat/completions`
- Anthropic Messages-compatible: `POST /v1/messages`

## Quick Connectivity Check

```bash
curl https://api.nusabase.io/v1/models \
  -H "Authorization: Bearer sk-your_api_key"
```

## Tool Guides

- [Integrate Claude Code](/en/guides/agent-integrations-claude-code)
- [Integrate GitHub Copilot](/en/guides/agent-integrations-github-copilot)
- [Integrate GitHub Copilot CLI](/en/guides/agent-integrations-github-copilot-cli)
- [Integrate Kilo Code](/en/guides/agent-integrations-kilo-code)
- [Integrate WorkBuddy/CodeBuddy](/en/guides/agent-integrations-workbuddy-codebuddy)
- [Integrate OpenCode](/en/guides/agent-integrations-opencode)
- [Integrate OpenClaw](/en/guides/agent-integrations-openclaw)
- [Integrate Hermes](/en/guides/agent-integrations-hermes)
- [Integrate CC Switch](/en/guides/agent-integrations-ccswitch) — graphical configuration for all tools above

## References

- [First Request Example](/en/getting-started/first-request)
- [Models and Pricing](/en/getting-started/pricing)
- [Error Codes](/en/getting-started/error-codes)
- [Claude Code LLM gateway](https://code.claude.com/docs/en/llm-gateway)
- [GitHub Copilot CLI BYOK](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/use-byok-models)
