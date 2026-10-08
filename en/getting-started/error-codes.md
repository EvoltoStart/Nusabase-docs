---
title: "Error Codes"
description: "Response format, HTTP status codes, platform error codes, and handling guidance for failed API calls."
---

This page explains common error responses for model calls and compatibility entry points. When troubleshooting, check both `HTTP status` and `error.code`: the status describes the protocol-level failure, while `error.code` identifies the platform-side failure reason written by the service.

## Error Response Format

Relay APIs usually return an OpenAI-compatible error body:

```json
{
  "error": {
    "message": "Error message",
    "type": "nusabase_api_error",
    "param": "",
    "code": "invalid_request"
  }
}
```

Field reference:

| Field | Description |
| --- | --- |
| `message` | User-facing error message. Some internal errors are masked as a generic server-busy message. |
| `type` | Error source or compatibility type, such as `nusabase_api_error`, `openai_error`, `claude_error`, or `upstream_error`. |
| `param` | Related parameter. Some endpoints omit this field. |
| `code` | Platform or upstream error code. This page only lists platform codes that are explicitly written in this project. |

<Note>
Some non-relay management APIs use a top-level `code`, `message`, `data`, and `dateTime` response shape. For model relay, OpenAI-compatible, Claude-compatible, and Gemini-compatible entry points, use the `error` shape on this page first.
</Note>

## HTTP Status Codes

| HTTP status | Common cause | What to do |
| --- | --- | --- |
| `400` | Invalid request body, invalid parameters, unsupported model input modality, or invalid channel selection. | Check JSON, required fields, model name, input modality, and endpoint path. |
| `401` | Missing API key, or the API key is invalid, expired, exhausted, or malformed. | Follow [Authentication](/en/getting-started/authentication) and check `Authorization: Bearer sk-...` or the compatible auth header. |
| `403` | Account or API access is disabled, IP allowlist mismatch, group access denied, or insufficient balance. | Check account status, API key permissions, IP restrictions, available groups, and balance. |
| `404` | The requested task, conversation, message, or resource does not exist. | Confirm the resource ID belongs to the current account and was created successfully. |
| `408` | The selected channel timed out. | Retry later, or switch models if the error persists. |
| `413` | Request body is too large or could not be read. | Reduce input size, file size, or context length. |
| `429` | Request rate exceeds the current rate-limit configuration. | Lower concurrency or request rate, and retry with exponential backoff. |
| `500` | Internal processing failed, request conversion failed, upstream response parsing failed, or quota pre-consumption failed. | Avoid high-frequency retries; keep the message and request ID for support. |
| `502` | Proxy or upstream resource fetching failed. | Retry later. For video proxy errors, confirm the task is complete and the result URL is valid. |
| `503` | The model or channel is unavailable, or system load protection was triggered. | Retry later or switch to another available model. |

## Platform Error Codes

These values come from the project's `types.ErrorCode` definitions and fixed error codes written by the system performance guard.

### Request And Parameters

| `error.code` | Meaning | What to do |
| --- | --- | --- |
| `invalid_request` | The request is invalid, such as missing parameters, invalid format, or an incompatible resource state. | Check endpoint path, request body, model name, and task/conversation IDs. |
| `read_request_body_failed` | The request body could not be read or exceeded the size limit. | Reduce request size and ensure the client does not disconnect early. |
| `bad_request_body` | The request body could not be prepared for the target channel. | Check JSON structure, field types, and multimodal input format. |
| `convert_request_failed` | The platform failed to convert the request to the upstream format. | Check whether the selected model supports this endpoint and input. |
| `reasoning_disable_not_supported` | The selected model does not support disabling reasoning. | Remove the reasoning-disable parameter or switch to a compatible model. |

### Authentication, Access, And Quota

| `error.code` | Meaning | What to do |
| --- | --- | --- |
| `access_denied` | The request was rejected by an access rule, such as an IP allowlist mismatch. | Check API key IP restrictions, group permissions, and account status. |
| `insufficient_user_quota` | The user or API key does not have enough available quota. | Top up, increase quota, or switch to a lower-cost model. |
| `pre_consume_token_quota_failed` | Quota pre-consumption failed before the relay call. | Check balance and API key status; contact support if balance is sufficient. |

### Model, Channel, And Routing

| `error.code` | Meaning | What to do |
| --- | --- | --- |
| `model_not_found` | No available channel exists for the selected model, or the model is unavailable in the current group. | Check model name, group access, and model availability. |
| `get_channel_failed` | Failed to get an available channel. | Retry later. If you selected a specific channel, check channel ID and status. |
| `gen_relay_info_failed` | Failed to generate relay request information. | Check request path, model name, and compatibility protocol. |
| `invalid_api_type` | The request could not be mapped to a valid API type. | Confirm that the endpoint path is supported by the documentation. |
| `model_price_error` | Failed to get or calculate model pricing. | Check whether the model is configured in pricing; contact support if it persists. |
| `channel:no_available_key` | The selected channel has no available upstream key. | Retry later or switch models. |
| `channel:param_override_invalid` | Channel parameter override configuration is invalid. | This usually requires a platform-side channel configuration fix. |
| `channel:header_override_invalid` | Channel header override configuration is invalid. | This usually requires a platform-side channel configuration fix. |
| `channel:model_mapped_error` | Failed to map the platform model to an upstream model. | Check model name and channel mapping configuration. |
| `channel:aws_client_error` | AWS channel client creation or configuration failed. | This usually requires checking platform-side AWS channel configuration. |
| `channel:invalid_key` | The upstream channel key is invalid. | This usually requires replacing or fixing the upstream key. |
| `channel:response_time_exceeded` | The channel response time exceeded the configured limit. | Retry later or switch to another model/channel. |

### Upstream Request And Response

| `error.code` | Meaning | What to do |
| --- | --- | --- |
| `do_request_failed` | Failed to call the upstream service. | Retry later; contact support with the request ID if it persists. |
| `read_response_body_failed` | Failed to read the upstream response body. | Retry later and avoid disconnecting a streaming client early. |
| `bad_response_status_code` | The upstream service returned an unexpected HTTP status. | Check the upstream status in `message` and decide whether to retry. |
| `bad_response` | The upstream response was not in the expected shape. | Retry later; switch models or contact support if it persists. |
| `bad_response_body` | Failed to parse the upstream response body. | Retry later; contact support if it persists. |
| `empty_response` | The upstream service returned an empty response. | Retry later. |
| `aws_invoke_error` | AWS model invocation failed. | Decide whether to retry based on the returned status; contact support if it persists. |
| `json_marshal_failed` | The platform failed to build a JSON response. | Contact support with the request ID. |

### Safety

| `error.code` | Meaning | What to do |
| --- | --- | --- |
| `sensitive_words_detected` | The input matched sensitive-word or safety rules. | Revise the input and retry. |
| `prompt_blocked` | The prompt was blocked by platform or upstream safety rules. | Remove disallowed content and retry. |
| `violation_fee.grok.csam` | Grok-related violation-fee marker. | Stop the related request pattern and review the input content. |

### Data And System

| `error.code` | Meaning | What to do |
| --- | --- | --- |
| `count_token_failed` | Token counting failed. | Shorten or simplify the input and retry. |
| `query_data_error` | Failed to query platform data. | Retry later; contact support if it persists. |
| `update_data_error` | Failed to update platform data. | Avoid duplicate submissions and confirm state before retrying. |
| `system_cpu_overloaded` | CPU usage exceeded the platform protection threshold. | Retry later. |
| `system_memory_overloaded` | Memory usage exceeded the platform protection threshold. | Retry later. |
| `system_disk_overloaded` | Disk usage exceeded the platform protection threshold. | Retry later. |

## Compatible Endpoint `error.type`

Some compatible endpoints do not return `error.code` and only mark the error category through `error.type`. Current video proxy endpoints use:

| `error.type` | Scenario | What to do |
| --- | --- | --- |
| `invalid_request_error` | Missing task ID, task not found, task not complete, or invalid request parameters. | Check task ID, task status, and request parameters. |
| `server_error` | Server-side failure while querying tasks, loading channel data, or proxying upstream video content. | Retry later; contact support if it persists. |

## Retry Guidance

1. `400`, `401`, `403`, `404`, and `413` usually require fixing the request, access, or quota first. Do not blindly retry them.
2. `408`, `429`, `500`, `502`, and `503` can be retried with exponential backoff and a maximum retry count.
3. `sensitive_words_detected`, `prompt_blocked`, and `violation_fee.grok.csam` require changing the input. Do not automatically retry the same request.
4. `insufficient_user_quota` requires topping up or reducing model cost before sending another request.
5. If `message` includes a request ID, include it when reporting the issue so logs can be located quickly.

Next, see [Authentication](/en/getting-started/authentication) for auth failures, or [First Request Example](/en/getting-started/first-request) for the minimal request format.

## References

- [Authentication](/en/getting-started/authentication)
- [First Request Example](/en/getting-started/first-request)
- [Compatibility Formats](/en/getting-started/compatibility)
