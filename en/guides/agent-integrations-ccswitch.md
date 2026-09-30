---
title: "Integrate CC Switch"
description: "Use CC Switch to configure Nusabase API for Claude Code, Codex, OpenCode, and other AI coding tools through a graphical interface."
---

CC Switch is a cross-platform desktop app for adding, enabling, and switching Providers for tools such as Claude Code, Codex, Gemini CLI, OpenCode, OpenClaw, and Hermes. This guide follows the CC Switch quickstart flow and adds the Nusabase Endpoint values each tool should use.

<Warning>
Different tools use different protocols, so their Nusabase Endpoints are different. Claude Code's Anthropic Messages setup uses the root URL `https://api.nusabase.io`; OpenAI-compatible setups use `https://api.nusabase.io/v1`. If you use CC Switch's Universal Provider, only select tools that share the same Endpoint type. When configuring Claude Code together with OpenAI-compatible tools, add separate Providers per tool.
</Warning>

## When To Use This

- You want to manage Nusabase API keys and model settings in CC Switch.
- You use multiple AI coding tools such as Claude Code, Codex, OpenCode, OpenClaw, and Hermes.
- You prefer switching Providers in a graphical interface instead of editing JSON/TOML files by hand.
- You want CC Switch to fetch the Nusabase model list for you.

## Prerequisites

- CC Switch is installed.
- The target CLI tools are installed and have been launched at least once so they create their default config directories.
- You have a Nusabase API key: `sk-...`.
- Your machine can reach `https://api.nusabase.io`.
- The example model `qwen3.5-flash` is only for demonstration; use `/v1/models` as the source of truth for your account.

## Install CC Switch

| Platform | Method |
| --- | --- |
| Windows | Download the `.msi` installer or portable `.zip` from [GitHub Releases](https://github.com/farion1231/cc-switch/releases). |
| macOS | Use `brew install --cask cc-switch`, or download the `.dmg` from Releases. |
| Linux | Download a `.deb`, `.rpm`, or `.AppImage`; Arch users can use `paru -S cc-switch-bin`. |

After installing, open CC Switch. On the left side of the interface, select Tools, and on the right side, manage the Provider for this tool. Click the **+** in the top right corner to add a new Provider.
## Verify Nusabase First

Before configuring CC Switch, confirm that your API key and model access work.

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

## Quick Setup Flow

1. At the top of the CC Switch interface, choose the tool you want to configure, like **Claude Code**, **Codex**, or **OpenCode**.2. Click the **+** button in the top-right corner.
3. Choose the right preset: **Custom** is typical for Claude Code; **OpenAI Compatible** or **Custom** is typical for OpenAI-compatible tools.
4. Fill in the Provider name, API key, Endpoint URL, and model.
5. If available, click the **Fetch Models** button next to the model field to pull models from Nusabase `/v1/models`.
6. Click **Add**.
7. Click **Enable** on the Provider card.
8. Restart the terminal or reopen the CLI as required by the tool, then send a test message.

When adding a Provider, focus on **API Key** and **Request URL**. Choose the request URL from the table below.

![CC Switch add provider screen](/images/ccswitch_en.png)

## Nusabase Settings Table

| Tool | CC Switch Tool | Suggested Preset | Endpoint URL | Notes |
| --- | --- | --- | --- | --- |
| Claude Code | Claude Code | Custom | `https://api.nusabase.io` | Uses Anthropic Messages. Use the root URL, without `/v1`. |
| Codex | Codex | Custom | `https://api.nusabase.io/v1` | Codex still sends `/responses`; enable local routing in CC Switch advanced options and set the upstream format to `Chat Completions`. |
| OpenCode | OpenCode | OpenAI Compatible or Custom | `https://api.nusabase.io/v1` | Uses OpenAI-compatible Chat Completions. |
| OpenClaw | OpenClaw | OpenAI Compatible or Custom | `https://api.nusabase.io/v1` | Uses OpenAI-compatible Chat Completions. |
| Hermes | Hermes | OpenAI Compatible or Custom | `https://api.nusabase.io/v1` | OpenAI-compatible mode uses the `/v1` Base URL. |

Common fields:

| Field | Value |
| --- | --- |
| Name | `Nusabase` |
| API Key | `sk-your_api_key` |
| Model | `qwen3.5-flash`, or a model returned by `/v1/models` |

<Note>
Do not paste a full endpoint path such as `https://api.nusabase.io/v1/chat/completions` into Endpoint URL. CC Switch and the target tool append the protocol-specific path.
</Note>

## Claude Code Notes

Claude Code appends `/v1/messages` to `ANTHROPIC_BASE_URL`, so the Endpoint URL must be the root URL:

```text
https://api.nusabase.io
```

Recommended settings:

| Field | Value | Notes |
| --- | --- | --- |
| API Format | Anthropic Messages | Keep the default Anthropic Messages format. |
| API Key | `sk-your_api_key` | CC Switch usually writes `ANTHROPIC_API_KEY`; Nusabase supports `x-api-key` authentication. |
| Endpoint URL | `https://api.nusabase.io` | Use the root URL, without `/v1`. |
| Model | `qwen3.5-flash` | Verify with `/v1/models`. |

If you manually edit `~/.claude/settings.json`, the usual shape is:

```json
{
  "env": {
    "ANTHROPIC_API_KEY": "sk-your_api_key",
    "ANTHROPIC_BASE_URL": "https://api.nusabase.io"
  }
}
```

You can also use `ANTHROPIC_AUTH_TOKEN` if you want Claude Code to send authentication as `Authorization: Bearer`.

## Codex Notes

Codex clients call the OpenAI Responses API. When connecting Codex to Nusabase through CC Switch, enable CC Switch local routing and set the Provider's upstream format to `Chat Completions`. CC Switch will convert Codex `/responses` requests into Nusabase OpenAI-compatible Chat Completions requests.

Set Endpoint URL to:

```text
https://api.nusabase.io/v1
```

Recommended settings:

| Field | Value | Notes |
| --- | --- | --- |
| API Key | `sk-your_api_key` | Used as Codex's OpenAI API key. |
| Endpoint URL | `https://api.nusabase.io/v1` | CC Switch local routing receives Codex `/responses` requests. |
| Model | `qwen3.5-flash` | Verify with `/v1/models`. |
| Needs Local Routing | On | Send Codex requests through the CC Switch local routing service first. |
| Upstream Format | `chat/completions` | Select Chat Completions to avoid calling `/v1/responses` upstream directly. |

<Warning>
Do not use `/v1/responses` as the direct upstream format for the CC Switch Codex Provider. Direct Responses routing may fail with `unexpected status 503 Service Unavailable: CC Switch local proxy failed while handling Codex endpoint /responses ... upstream_status: HTTP 503; cause: DistributorGetChannelFailed`. Enable local routing and set the upstream format to `chat/completions` instead.
</Warning>

### Codex Local Routing

Local routing is recommended for Codex with Nusabase. It is useful when:

- You want CC Switch to convert Codex `/responses` requests into Nusabase's more stable `/chat/completions` path.
- You want all Codex requests to pass through CC Switch and appear in its routing logs.
- You are debugging Provider switching or request forwarding and need to observe the proxy path.

Setup:

1. Open CC Switch **Settings → Advanced → Routing Service**.
2. Start the routing service. The default listener is `127.0.0.1:15721`.
3. In **App Routing**, enable the **Codex** routing toggle.
4.    ![CC Switch Configure application routing](/images/ccswitch_router_en1.png)
4. In the Codex Nusabase Provider advanced options, set the Upstream Format to "Chat Completions".
5.    ![CC Switch Select upstream format](/images/ccswitch_router_en2.png)
5. Keep Endpoint URL as `https://api.nusabase.io/v1`.

When converted to Chat Completions, the path is:

```text
Codex CLI
  -> http://127.0.0.1:15721/v1
  -> CC Switch local routing
  -> https://api.nusabase.io/v1/chat/completions
```

## OpenCode / OpenClaw / Hermes Notes

When these tools use OpenAI-compatible Chat Completions, set Endpoint URL to:

```text
https://api.nusabase.io/v1
```

Common settings:

| Field | Value |
| --- | --- |
| Name | `Nusabase` |
| API Key | `sk-your_api_key` |
| Endpoint URL | `https://api.nusabase.io/v1` |
| Model | `qwen3.5-flash` |

After configuration, close and reopen the terminal, then start the target tool.

## Fetch Models

CC Switch can fetch models from the Provider's model endpoint. After filling in API Key and Endpoint URL, click the **Fetch Models** button next to the model field.

Common failures:

| Error | Likely Cause | Fix |
| --- | --- | --- |
| 401/403 | Invalid API key or extra whitespace | Re-paste the key and confirm it starts with `sk-`. |
| 404/405 | Endpoint URL is wrong | Use the root URL for Claude Code; use the `/v1` URL for OpenAI-compatible tools. |
| Timeout | Network unreachable or high latency | Verify the network with `curl /v1/models`, then retry. |

## Apply And Verify

Provider switching behavior varies by tool:

| Tool | How Changes Take Effect |
| --- | --- |
| Claude Code | Usually supports hot reload; closing the current session and rerunning `claude` is more reliable. |
| Codex | Close and reopen the terminal, then run `codex`. |
| OpenCode / OpenClaw | Close and reopen the terminal, then run the target CLI. |
| Hermes | Restart the terminal, then run `hermes`. |

Success criteria:

- The Nusabase Provider is enabled in CC Switch.
- The tool's model picker or `/model` command shows the Nusabase model.
- A test message returns normally.
- If you use Codex local routing, CC Switch's routing panel shows proxied request logs.

## Troubleshooting

| Problem | Likely Cause | Fix |
| --- | --- | --- |
| Provider switch does not take effect | The target CLI is still using the old process or old environment variables | Close and reopen the terminal, then start the tool again. |
| `401` or `403` | API key is wrong, expired, or contains extra whitespace | Re-edit the Provider and confirm the API key starts with `sk-` and has no whitespace. |
| `404` | Endpoint URL type is wrong | Claude Code uses `https://api.nusabase.io`; OpenAI-compatible tools use `https://api.nusabase.io/v1`. |
| Codex says the routing service is required | The Provider has Needs Local Routing enabled, but the routing service is not running | Start the CC Switch routing service and confirm Codex is enabled in App Routing. |
| Codex local proxy connection fails | Routing service is not running, the port is occupied, or app routing is disabled | Check **Settings → Advanced → Routing Service** and confirm `127.0.0.1:15721` is available. |
| Codex `/v1/responses` returns `unexpected status 503 Service Unavailable` with `CC Switch local proxy failed while handling Codex endpoint /responses` and `DistributorGetChannelFailed` | The Codex Provider is using Responses as the direct upstream format, or local routing was not switched to Chat Completions | Enable **Needs Local Routing** and set **Upstream Format** to `Chat Completions` in the Provider advanced options. |
| Model list is empty | Endpoint URL or API key is wrong | Verify with curl that `/v1/models` returns models. |
| Non-Claude models behave unexpectedly in Claude Code | Claude Code adds attribution metadata | Enable **Hide Attribution** in Provider advanced options, or set `CLAUDE_CODE_ATTRIBUTION_HEADER=0`. |

See [Models and Pricing](/en/getting-started/pricing) for current models, context windows, and pricing.

## References

- [CC Switch Quickstart](https://ccswitch.io/docs?section=getting-started&item=quickstart)
- [First Request Example](/en/getting-started/first-request)
- [Models and Pricing](/en/getting-started/pricing)
- [Error Codes](/en/getting-started/error-codes)
