---
title: "Models and Pricing"
description: "USD pricing reference for text, image, video, multimodal, audio, realtime, and third-party models."
---

This page summarizes the USD pricing collected for the API aggregation platform. The prices in the source document currently do not cover batch inference or offline inference.

Actual billing depends on the selected model, input length, output length, resolution, video duration, audio duration, cache usage, and modality.

## Billing Units

| Model type | Common billing unit | Notes |
| --- | --- | --- |
| Language models | USD / million tokens | Usually split into input, output, cache hit, cache write, cache read, and cache storage. |
| Multimodal models | USD / million tokens | Text, image, video, and audio can use different input/output prices. |
| Image models | USD / image or USD / million tokens | Some models expose both token-based and image-count-based pricing. |
| Video models | USD / second or USD / million tokens | Price depends on resolution, audio, video input, and editing mode. |
| Text-to-speech | USD / 10k characters | Output audio is usually not separately charged. |
| Speech-to-text | USD / second | Output text is usually not separately charged. |
| Realtime audio | USD / million tokens or USD / minute | OpenAI realtime models may price audio, text, and image inputs separately. |

`/` means the source sheet did not provide a separate price, the field is not applicable, or the capability is not available.

## Covered Providers

| Region | Provider | Model groups |
| --- | --- | --- |
| Global | Volcano Engine | `Seedance 2.0`, `seedance 2.0 fast`, `seedance-1.5-pro`, `seedream 5.0 lite`, `seedream 4.5`, `seedream 4.0`, `Doubao-Seed-2.0`, `Doubao-Seed-Character`, `Doubao TTS 2.0`, `GLM-4.7`, `DeepSeek-V3.2` |
| Global | Alibaba Cloud | `Wan 2.5 / 2.6 / 2.7`, `qwen3.5 / qwen3.6`, `happyhorse-1.0`, `Qwen-Image-2.0`, `Qwen-3.5-Max`, `gui-plus-2026-02-26`, `qwen-flash-character-2026-02-26`, `z-image-turbo`, `qwen3-tts-flash`, `qwen3-asr-flash`, `GLM-5.1`, `kimi-k2.5`, `kimi-k2.6`, `DeepSeek-V3.2`, `DeepSeek-V4-pro`, `deepseek-v4-flash`, `MiniMax-M2.5` |
| Global | MiniMax | `MiniMax-M2.7`, `MiniMax-Speech` series, pending completion |
| Global | Kling | `Kling-V3-Omni` |
| Global | Vidu | `Vidu Q3` series |
| Global | Xiaomi | `mimo-v2.5`, `mimo-v2.5-pro`, `mimo-v2-pro` |
| Global | OpenAI | `GPT-5.4`, `GPT-Realtime` series, `GPT-Image-2` |
| Global | Google | `Nano banana Pro`, `Nano banana 2`, `Veo 3.1` series |
| Global | X | `Grok` series, incomplete in source sheet |
| Global | Suno | `suno-v3`, `suno-v3.5` |
| Global | ElevenLabs | `Eleven v3` |

## Language Models

Prices are in USD / million tokens unless otherwise stated.

| Provider | Model | Context / range | Input | Output | Cache / extra pricing | RPM | Notes |
| --- | --- | --- | ---: | ---: | --- | ---: | --- |
| MiniMax | `MiniMax-M2.7` | / | 0.3000 | 1.2000 | Cache write 0.3750, cache read 0.0600 | / |  |
| MiniMax | `MiniMax-M2.7-highspeed` | / | 0.6000 | 2.4000 | Cache write 0.3750, cache read 0.0600 | / |  |
| Xiaomi | `mimo-v2.5-pro` | `(0, 256]k` | 1.0000 | 3.0000 | Cache hit 0.2000 | / | Cache write is temporarily free as of 2026-05-21 |
| Xiaomi | `mimo-v2.5-pro` | `(256, 1024]k` | 2.0000 | 6.0000 | Cache hit 0.4000 | / |  |
| Xiaomi | `mimo-v2-pro` | `(0, 256]k` | 1.0000 | 3.0000 | Cache hit 0.2000 | / |  |
| Xiaomi | `mimo-v2-pro` | `(256, 1024]k` | 2.0000 | 6.0000 | Cache hit 0.4000 | / |  |
| Xiaomi | `mimo-v2.5` | `(0, 256]k` | 0.4000 | 2.0000 | Cache hit 0.0800 | / |  |
| Xiaomi | `mimo-v2.5` | `(256, 1024]k` | 0.8000 | 4.0000 | Cache hit 0.1600 | / |  |
| OpenAI | `GPT-5.4` | Below 272k | 2.5000 | 15.0000 | Cache hit 0.2500 | / | Output price includes visible output tokens and hidden reasoning tokens. Data residency endpoints add 10%. |
| OpenAI | `GPT-5.4` | Above 272k | 5.0000 | 22.5000 | Cache hit 0.5000 | / | Output price includes visible output tokens and hidden reasoning tokens. |
| X | `grok-4.3` | / | 1.2500 | 2.5000 | Cache hit 0.2000 | 1,800 | 1M context window |
| X | `grok-4.20-multi-agent-0309` | / | 1.2500 | 2.5000 | Cache hit 0.2000 | 1,800 | 2M context window |
| X | `grok-4.20-0309-reasoning` | / | 1.2500 | 2.5000 | Cache hit 0.2000 | 1,800 | 1M context window |
| X | `grok-4.20-0309-non-reasoning` | / | 1.2500 | 2.5000 | Cache hit 0.2000 | 1,800 | 1M context window |
| Alibaba Cloud | `qwen3-max-preview` | `(0, 32]k` | 1.2000 | 6.0000 | Cache hit 0.2400 | 600 | Input cache hits are charged at 20%. |
| Alibaba Cloud | `qwen3-max-preview` | `(32, 128]k` | 2.4000 | 12.0000 | Cache hit 0.4800 | 600 | Input cache hits are charged at 20%. |
| Alibaba Cloud | `qwen3-max-preview` | `(128, 256]k` | 3.0000 | 15.0000 | Cache hit 0.6000 | / |  |
| Alibaba Cloud | `qwen3.6-max-preview` | `(0, 128]k` | 1.3000 | 7.8000 | Explicit cache hit 0.1300, explicit cache creation 1.6250 | 600 |  |
| Alibaba Cloud | `qwen3.6-max-preview` | `(128, 256]k` | 2.0000 | 12.0000 | Explicit cache hit 0.2000, explicit cache creation 2.5000 | / |  |
| Alibaba Cloud | `qwen3.6-plus` | `(0, 256]k` | 0.5000 | 3.0000 | Explicit cache hit 0.0500, explicit cache creation 0.6250 | 15,000 | Equivalent to `qwen3.6-plus-2026-04-02` |
| Alibaba Cloud | `qwen3.6-plus` | `(256, 1024]k` | 2.0000 | 6.0000 | Explicit cache hit 0.2000, explicit cache creation 2.5000 | / |  |
| Alibaba Cloud | `qwen3.6-plus-2026-04-02` | `(0, 256]k` | 0.5000 | 3.0000 | / | 60 | Snapshot model |
| Alibaba Cloud | `qwen3.6-plus-2026-04-02` | `(256, 1024]k` | 2.0000 | 6.0000 | / | / | Snapshot model |
| Alibaba Cloud | `qwen3.5-plus` | `(0, 256]k` | 0.4000 | 2.4000 | Explicit cache hit 0.0400, explicit cache creation 0.5000 | 15,000 | Equivalent to `qwen3.5-plus-2026-02-15` |
| Alibaba Cloud | `qwen3.5-plus` | `(256, 1024]k` | 0.5000 | 3.0000 | Explicit cache hit 0.0500, explicit cache creation 0.6250 | / |  |
| Alibaba Cloud | `qwen3.5-plus-2026-04-20` | `(0, 256]k` | 0.4000 | 2.4000 | Explicit cache hit 0.0400, explicit cache creation 0.5000 | 600 | Snapshot model |
| Alibaba Cloud | `qwen3.5-plus-2026-04-20` | `(256, 1024]k` | 0.5000 | 3.0000 | Explicit cache hit 0.0500, explicit cache creation 0.6250 | / | Snapshot model |
| Alibaba Cloud | `qwen3.5-plus-2026-02-15` | `(0, 256]k` | 0.4000 | 2.4000 | / | 60 | Snapshot model |
| Alibaba Cloud | `qwen3.5-plus-2026-02-15` | `(256, 1024]k` | 0.5000 | 3.0000 | / | / | Snapshot model |
| Alibaba Cloud | `qwen3.6-flash` | `(0, 256]k` | 0.2500 | 1.5000 | Explicit cache hit 0.0250, explicit cache creation 0.3125 | 15,000 | Equivalent to `qwen3.6-flash-2026-04-16` |
| Alibaba Cloud | `qwen3.6-flash` | `(256, 1024]k` | 1.0000 | 4.0000 | Explicit cache hit 0.1000, explicit cache creation 1.2500 | / |  |
| Alibaba Cloud | `qwen3.6-flash-2026-04-16` | `(0, 256]k` | 0.2500 | 1.5000 | / | 60 | Snapshot model |
| Alibaba Cloud | `qwen3.6-flash-2026-04-16` | `(256, 1024]k` | 1.0000 | 4.0000 | / | / | Snapshot model |
| Alibaba Cloud | `qwen3.5-flash` | / | 0.1000 | 0.4000 | Explicit cache hit 0.0100, explicit cache creation 0.1250 | 15,000 |  |
| Alibaba Cloud | `qwen3.5-flash-2026-02-23` | / | 0.1000 | 0.4000 | / | 60 | Snapshot model |
| Volcano Engine | `doubao-seed-2.0-pro` | `(0, 128]k` | 0.5000 | 3.0000 | Cache storage 0.0083, cache hit 0.1000 | / |  |
| Volcano Engine | `doubao-seed-2.0-pro` | `(128, 256]k` | 1.0000 | 6.0000 | Cache storage 0.0083, cache hit 0.2000 | / |  |
| Volcano Engine | `doubao-seed-2.0-lite` | `(0, 128]k` | 0.2500 | 2.0000 | Cache storage 0.0083, cache hit 0.0500 | / |  |
| Volcano Engine | `doubao-seed-2.0-lite` | `(128, 256]k` | 0.5000 | 4.0000 | Cache storage 0.0083, cache hit 0.1000 | / |  |
| Volcano Engine | `doubao-seed-2.0-mini` | `(0, 128]k` | 0.1000 | 0.4000 | Cache storage 0.0083, cache hit 0.0200 | / |  |
| Volcano Engine | `doubao-seed-2.0-mini` | `(128, 256]k` | 0.2000 | 0.8000 | Cache storage 0.0083, cache hit 0.0400 | / |  |
| Volcano Engine | `doubao-seed-2.0-code` | `[0, 32]k` | 0.4600 | 2.2900 | Cache storage 0.0024, cache hit 0.0914 | / | Converted from RMB pricing by dividing by 7 because overseas pricing was not available in the source. |
| Volcano Engine | `doubao-seed-2.0-code` | `(32, 128]k` | 0.6900 | 3.4300 | Cache storage 0.0024, cache hit 0.1371 | / | Converted from RMB pricing by dividing by 7. |
| Volcano Engine | `doubao-seed-2.0-code` | `(128, 256]k` | 1.3700 | 6.8600 | Cache storage 0.0024, cache hit 0.2743 | / | Converted from RMB pricing by dividing by 7. |
| Volcano Engine | `Doubao-Seed-Character` | `[0, 32]k` | 0.1100 | 0.2900 | Cache storage 0.0024, cache hit 0.0229 | / | Converted from RMB pricing by dividing by 7. |
| Volcano Engine | `Doubao-Seed-Character` | `(32, 128]k` | 0.1700 | 0.8600 | Cache storage 0.0024, cache hit 0.0229 | / | Converted from RMB pricing by dividing by 7. |

## Third-party Open-weight Language Models

Prices are in USD / million tokens unless otherwise stated.

| Model | Provider | Context / range | Input | Output | Cache / extra pricing | Notes |
| --- | --- | --- | ---: | ---: | --- | --- |
| `glm-4.7` | Volcano Engine | / | 0.6000 | 2.2000 | Cache storage 0.0083, explicit cache creation 0.1100 |  |
| `glm-5.1` | Bailian | `(0, 32]k` | 0.825 | 3.301 | / | Non-thinking and thinking modes. Output includes chain-of-thought and answer. Free quota: 1M tokens for 90 days. |
| `glm-5.1` | Bailian | `(32, 200]k` | 1.100 | 3.851 | / | Non-thinking and thinking modes. |
| `kimi-k2.5` | Bailian | / | 0.574 | 3.011 | / |  |
| `kimi-k2.6` | Bailian | / | 0.8939 | 3.7131 | / |  |
| `DeepSeek-V3.2` | Volcano Engine | `(0, 32]k` | 0.2800 | 0.4200 | Cache storage 0.0083, cache creation 0.0560 |  |
| `DeepSeek-V3.2` | Volcano Engine | `(32, 128]k` | 0.5600 | 0.8400 | Cache storage 0.0083, cache creation 0.0560 |  |
| `DeepSeek-V3.2` | Bailian | / | 0.5700 | 1.7100 | Explicit cache creation 0.7130, implicit cache hit 0.1140, explicit cache hit 0.0570 | Non-thinking and thinking modes. Output includes chain-of-thought and answer. |
| `DeepSeek-V4-pro` | Bailian | / | 2.40 | 4.80 | Cache hit 0.20 |  |
| `deepseek-v4-flash` | Bailian | / | 0.20 | 0.40 | Cache hit 0.04 |  |
| `MiniMax-M2.5` | Bailian | / | 0.3040 | 1.2130 | / | Thinking mode only. Output includes chain-of-thought and answer. |

### Open-weight Qwen Series

| Provider | Model | Context / range | Input | Output |
| --- | --- | --- | ---: | ---: |
| Alibaba Cloud | `qwen3.5-397b-a17b` | `(0, 256]k` | 0.600 | 3.600 |
| Alibaba Cloud | `qwen3.5-27b` | `(0, 256]k` | 0.300 | 2.400 |
| Alibaba Cloud | `qwen3.6-35b-a3b` | `(0, 256]k` | 0.248 | 1.485 |
| Alibaba Cloud | `qwen3.6-27b` | `(0, 256]k` | 0.600 | 3.600 |
| Alibaba Cloud | `qwen3.5-122b-a10b` | `(0, 256]k` | 0.400 | 3.200 |
| Alibaba Cloud | `qwen3.5-35b-a3b` | `(0, 256]k` | 0.250 | 2.000 |

## Video Models

Video prices are listed in USD / second unless the unit column says USD / million tokens.

| Provider | Model | Audio | Resolution / mode | Input contains video | Price | Unit | Input price | Capability / notes |
| --- | --- | --- | --- | --- | ---: | --- | --- | --- |
| Google | `Veo 3.1` | Audio | 720p / 1080p | No | 0.400 | Second | / | Image-to-video, text-to-video |
| Google | `Veo 3.1` | Audio | 4K | No | 0.600 | Second | / | Image-to-video, text-to-video |
| Google | `Veo 3.1` | No audio | 720p / 1080p | No | 0.200 | Second | / | Image-to-video, text-to-video |
| Google | `Veo 3.1` | No audio | 4K | No | 0.400 | Second | / | Image-to-video, text-to-video |
| Google | `Veo 3.1 Fast` | Audio | 720p | No | 0.100 | Second | / | Image-to-video, text-to-video |
| Google | `Veo 3.1 Fast` | Audio | 1080p | No | 0.120 | Second | / | Image-to-video, text-to-video |
| Google | `Veo 3.1 Fast` | Audio | 4K | No | 0.300 | Second | / | Image-to-video, text-to-video |
| Google | `Veo 3.1 Fast` | No audio | 720p | No | 0.080 | Second | / | Image-to-video, text-to-video |
| Google | `Veo 3.1 Fast` | No audio | 1080p | No | 0.100 | Second | / | Image-to-video, text-to-video |
| Google | `Veo 3.1 Fast` | No audio | 4K | No | 0.250 | Second | / | Image-to-video, text-to-video |
| Google | `Veo 3.1 Lite` | Audio | 720p | No | 0.050 | Second | / | Image-to-video, text-to-video |
| Google | `Veo 3.1 Lite` | Audio | 1080p | No | 0.080 | Second | / | Image-to-video, text-to-video |
| Google | `Veo 3.1 Lite` | No audio | 720p | No | 0.030 | Second | / | Image-to-video, text-to-video |
| Google | `Veo 3.1 Lite` | No audio | 1080p | No | 0.050 | Second | / | Image-to-video, text-to-video |
| X | `grok-imagine-video` | / | 480p | Yes, charged separately | 0.050 | Second | 0.01 USD / second and 0.002 USD / image | Image-to-video, text-to-video, video-to-video; 70 RPM |
| X | `grok-imagine-video` | / | 720p | Yes, charged separately | 0.070 | Second | Same as above | Image-to-video, text-to-video, video-to-video; 70 RPM |
| Volcano Engine | `Seedance 2.0` | / | 480p / 720p | No | 7.000 | Million tokens | / | Image-to-video, text-to-video, video editing |
| Volcano Engine | `Seedance 2.0` | / | 480p / 720p | Yes | 4.300 | Million tokens | / | Image-to-video, text-to-video, video editing |
| Volcano Engine | `Seedance 2.0` | / | 1080p | No | 7.700 | Million tokens | / | Image-to-video, text-to-video, video editing |
| Volcano Engine | `Seedance 2.0` | / | 1080p | Yes | 4.700 | Million tokens | / | Image-to-video, text-to-video, video editing |
| Volcano Engine | `seedance 2.0 fast` | / | 480p / 720p | No | 5.600 | Million tokens | / | Image-to-video, text-to-video, video editing |
| Volcano Engine | `seedance 2.0 fast` | / | 480p / 720p | Yes | 3.300 | Million tokens | / | Image-to-video, text-to-video, video editing |
| Volcano Engine | `seedance-1.5-pro` | Audio | 480p / 720p / 1080p | No | 2.400 | Million tokens | / | Image-to-video, text-to-video; 600 RPM |
| Volcano Engine | `seedance-1.5-pro` | No audio | 480p / 720p / 1080p | No | 1.200 | Million tokens | / | Image-to-video, text-to-video; 600 RPM |
| Alibaba Cloud | `wan2.6-i2v-flash` | Audio | 720p / 1080p | No | 0.0500 / 0.0750 | Second | / | Image-to-video based on first frame; 300 RPM |
| Alibaba Cloud | `wan2.6-i2v-flash` | No audio | 720p / 1080p | No | 0.0250 / 0.0375 | Second | / | Image-to-video based on first frame; 300 RPM |
| Alibaba Cloud | `wan2.6-r2v-flash` | Audio | 720p / 1080p | Yes | 0.0500 / 0.0750 | Second | / | Reference-to-video; 300 RPM |
| Alibaba Cloud | `wan2.6-r2v-flash` | No audio | 720p / 1080p | Yes | 0.0250 / 0.0375 | Second | / | Reference-to-video; 300 RPM |
| Alibaba Cloud | `wan2.6-i2v` | Audio | 720p / 1080p | No | 0.1000 / 0.1500 | Second | / | Image-to-video based on first frame; 300 RPM |
| Alibaba Cloud | `wan2.6-r2v` | No audio | 720p / 1080p | Yes | 0.1000 / 0.1500 | Second | / | Reference-to-video; 300 RPM |
| Alibaba Cloud | `wan2.6-t2v` | No audio | 720p / 1080p | No | 0.1000 / 0.1500 | Second | / | Text-to-video; 300 RPM |
| Alibaba Cloud | `wan2.5-i2v-preview` | No audio | 480p / 720p / 1080p | No | 0.0500 / 0.1000 / 0.1500 | Second | / | Image-to-video; 300 RPM |
| Alibaba Cloud | `wan2.5-t2v-preview` | No audio | 480p / 720p / 1080p | No | 0.0500 / 0.1000 / 0.1500 | Second | / | Text-to-video; 300 RPM |
| Alibaba Cloud | `wan2.7-r2v` | No audio | 720p / 1080p | Yes | 0.1000 / 0.1500 | Second | / | Reference-to-video, supports image, text, audio, and video input; 300 RPM |
| Alibaba Cloud | `wan2.7-i2v-2026-04-25`, `wan2.7-i2v` | No audio | 720p / 1080p | No | 0.1000 / 0.1500 | Second | / | Image-to-video, supports image, text, and audio input; 300 RPM |
| Alibaba Cloud | `wan2.7-t2v-2026-04-25`, `wan2.7-t2v` | No audio | 720p / 1080p | No | 0.1000 / 0.1500 | Second | / | Text-to-video, supports text and audio input; 300 RPM |
| Alibaba Cloud | `wan2.7-videoedit` | No audio | 720p / 1080p | Yes | 0.1000 / 0.1500 | Second | / | Video editing, supports image, text, and video input; 300 RPM |
| Alibaba Cloud | `happyhorse-1.0-t2v` | No audio | 720p / 1080p | No | 0.1400 / 0.2400 | Second | / | Text-to-video only; 20% limited-time discount on 2026.5.21; 300 RPM |
| Alibaba Cloud | `happyhorse-1.0-i2v` | No audio | 720p / 1080p | No | 0.1400 / 0.2400 | Second | / | Image-to-video; 20% limited-time discount on 2026.5.21; 300 RPM |
| Alibaba Cloud | `happyhorse-1.0-r2v` | No audio | 720p / 1080p | No | 0.1400 / 0.2400 | Second | / | Reference-to-video; 20% limited-time discount on 2026.5.21; 300 RPM |
| Alibaba Cloud | `happyhorse-1.0-video-edit` | No audio | 720p / 1080p | Yes | 0.1400 / 0.2400 | Second | / | Video editing; 20% limited-time discount on 2026.5.21; 300 RPM |
| Kling | `Kling-V3-Omni` | No audio | std / pro / 4K | No | 0.0840 / 0.1120 / 0.4200 | Second | / | Video generation |
| Kling | `Kling-V3-Omni` | Audio | std / pro / 4K | No | 0.1120 / 0.1400 / 0.4200 | Second | / | Video generation |
| Kling | `Kling-V3-Omni` | No audio | std / pro | Yes | 0.1260 / 0.1680 | Second | / | Video editing |
| Vidu | `Vidu Q3-pro` | / | 1080p / 720p / 540p | No | 0.1200 / 0.1000 / 0.0450 | Second | / | Image-to-video, text-to-video, first/last frame; 1-16 seconds |
| Vidu | `Vidu Q3-turbo` | / | 1080p / 720p / 540p | No | 0.6500 / 0.0550 / 0.0350 | Second | / | Image-to-video, text-to-video, first/last frame; 1-16 seconds |
| Vidu | `Vidu Q3-pro-fast` | / | 1080p / 720p | No | 0.1250 / 0.1000 | Second | / | Image-to-video; 1-16 seconds |
| Vidu | `Vidu Q3-mix` | / | 720p / 1080p | No | 0.1200 / 0.1450 | Second | / | Reference-to-video; 3-16 seconds |
| Vidu | `Vidu Q3-turbo` | / | 540p / 720p / 1080p | No | 0.0200 / 0.0500 / 0.0650 | Second | / | Reference-to-video; 3-16 seconds |
| Vidu | `Vidu Q3` | / | 540p / 720p / 1080p | No | 0.0350 / 0.0600 / 0.0750 | Second | / | Reference-to-video; 3-16 seconds |

## Image Models

Image models can be priced by tokens and/or images depending on the provider.

| Provider | Model | Input type | Output mode | Input USD / million tokens | Output USD / million tokens | Cached input | Input USD / image | Output USD / image | RPM / notes |
| --- | --- | --- | --- | ---: | ---: | ---: | ---: | ---: | --- |
| OpenAI | `GPT-Image-2` | Text | / | 5.000 | / | 1.250 | / | / |  |
| OpenAI | `GPT-Image-2` | Image | / | 8.000 | 30.000 | 2.000 | / | / |  |
| Google | `Nano banana Pro` | Text | / | 2.000 | 12.000 | / | / | / | Shared free quota: 5,000 search-enhanced calls per month across Gemini 3 models; then 14 USD / 1,000 calls. |
| Google | `Nano banana Pro` | Image | 1K / 2K | 2.000 | 120.000 | / | 0.001 | 0.134 |  |
| Google | `Nano banana Pro` | Image | 4K | 2.000 | 120.000 | / | 0.001 | 0.240 |  |
| Google | `Nano banana 2` | Text | / | 0.500 | 3.000 | / | / | / |  |
| Google | `Nano banana 2` | Image | 0.5K | 0.500 | 60.000 | / | / | 0.045 |  |
| Google | `Nano banana 2` | Image | 1K | 0.500 | 60.000 | / | / | 0.067 |  |
| Google | `Nano banana 2` | Image | 2K | 0.500 | 60.000 | / | / | 0.101 |  |
| Google | `Nano banana 2` | Image | 4K | 0.500 | 60.000 | / | / | 0.151 |  |
| X | `grok-imagine-image` | Text / image | 1K | / | / | / | 0.002 | 0.020 | 300 RPM |
| X | `grok-imagine-image` | Text / image | 2K | / | / | / | / | 0.020 |  |
| X | `grok-imagine-image-quality` | Text / image | 1K | / | / | / | 0.010 | 0.050 | 300 RPM |
| X | `grok-imagine-image-quality` | Text / image | 2K | / | / | / | / | 0.070 |  |
| Alibaba Cloud | `wan2.6-image` | Text / image | / | / | / | / | / | 0.030 | 300 RPM |
| Alibaba Cloud | `wan2.6-t2i` | Text | / | / | / | / | / | 0.030 | 300 RPM |
| Alibaba Cloud | `wan2.5-t2i-preview` | Text | / | / | / | / | / | 0.030 | 300 RPM |
| Alibaba Cloud | `z-image-turbo` | Text | Standard | / | / | / | / | 0.150 | 120 RPM |
| Alibaba Cloud | `z-image-turbo` | Text | Thinking | / | / | / | / | 0.030 |  |
| Alibaba Cloud | `wan2.7-image-pro` | Text / image | / | / | / | / | / | 0.075 | 300 RPM |
| Alibaba Cloud | `wan2.7-image` | Text / image | / | / | / | / | / | 0.030 | 300 RPM |
| Alibaba Cloud | `qwen-image-2.0-pro-2026-04-22` | Text / image | / | / | / | / | / | 0.075 | 2 RPM |
| Alibaba Cloud | `qwen-image-2.0` | Text / image | / | / | / | / | / | 0.035 | 120 RPM |
| Volcano Engine | `seedream 5.0 lite` | Text / image | / | / | / | / | / | 0.035 |  |
| Volcano Engine | `seedream 4.5` | Text / image | / | / | / | / | / | 0.040 |  |
| Volcano Engine | `seedream 4.0` | Text / image | / | / | / | / | / | 0.030 |  |

## Multimodal Models

Prices are in USD / million tokens.

| Provider | Model | Input: text / image / video | Input: audio | Output: text | Output: text + audio | Series | Notes |
| --- | --- | ---: | ---: | ---: | ---: | --- | --- |
| Alibaba Cloud | `qwen3.5-omni-plus` | 1.40 | 11.00 | 8.30 | 44.00 | Qwen Omni | Each 1M token free quota is valid for 90 days; equivalent to `qwen3.5-omni-plus-2026-03-15` |
| Alibaba Cloud | `qwen3.5-omni-plus-2026-03-15` | 1.40 | 11.00 | 8.30 | 44.00 | Qwen Omni | Snapshot model |
| Alibaba Cloud | `qwen3.5-omni-flash` | 0.40 | 3.00 | 2.20 | 11.90 | Qwen Omni | Equivalent to `qwen3.5-omni-flash-2026-03-15` |
| Alibaba Cloud | `qwen3.5-omni-flash-2026-03-15` | 0.40 | 3.00 | 2.20 | 11.90 | Qwen Omni | Snapshot model |
| Alibaba Cloud | `qwen3.5-omni-plus-realtime` | 2.10 | 16.50 | 12.40 | 62.00 | Qwen Omni-Realtime | Each 1M token free quota is valid for 90 days |
| Alibaba Cloud | `qwen3.5-omni-plus-realtime-2026-03-15` | 2.10 | 16.50 | 12.40 | 62.00 | Qwen Omni-Realtime | Snapshot model |
| Alibaba Cloud | `qwen3.5-omni-flash-realtime` | 0.55 | 4.50 | 3.30 | 17.70 | Qwen Omni-Realtime |  |
| Alibaba Cloud | `qwen3.5-omni-flash-realtime-2026-03-15` | 0.55 | 4.50 | 3.30 | 17.70 | Qwen Omni-Realtime | Snapshot model |

## Realtime and Audio Models

### Realtime conversation

| Provider | Model | Input type | Input price | Output price | Cached input | Output type |
| --- | --- | --- | ---: | ---: | ---: | --- |
| OpenAI | `GPT-Realtime-2` | Audio | 32.00 USD / million tokens | 64.00 USD / million tokens | 0.40 | Text, audio |
| OpenAI | `GPT-Realtime-2` | Text | 4.00 USD / million tokens | 24.00 USD / million tokens | 0.40 | Text, audio |
| OpenAI | `GPT-Realtime-2` | Image | 5.00 USD / million tokens | / | 0.50 | Text, audio |

### Text-to-speech

| Provider | Model | Price | Capability | Notes |
| --- | --- | ---: | --- | --- |
| ElevenLabs | `Eleven v3` | 1.00 USD / 10k characters | Text-to-speech |  |
| Alibaba Cloud | `qwen3-tts-flash-realtime` | 0.13 USD / 10k characters | Text-to-speech | Equivalent to `qwen3-tts-flash-realtime-2025-11-27` |
| Alibaba Cloud | `qwen3-tts-flash-realtime-2025-11-27` | 0.13 USD / 10k characters | Text-to-speech | Snapshot model |
| Alibaba Cloud | `qwen3-tts-flash-realtime-2025-09-18` | 0.13 USD / 10k characters | Text-to-speech | Snapshot model |
| Alibaba Cloud | `qwen3-tts-flash` | 0.10 USD / 10k characters | Text-to-speech | Equivalent to `qwen3-tts-flash-2025-11-27` |
| Alibaba Cloud | `qwen3-tts-flash-2025-11-27` | 0.10 USD / 10k characters | Text-to-speech | Snapshot model |
| Alibaba Cloud | `qwen3-tts-flash-2025-09-18` | 0.10 USD / 10k characters | Text-to-speech | Snapshot model |
| Volcano Engine | `Doubao-Text-to-Speech-2.0` | 0.4286 USD / 10k characters | Text-to-speech | Converted from RMB pricing because overseas pricing was unavailable due to account-region restrictions. |

### Speech-to-text

| Provider | Model | Price | Capability | Notes |
| --- | --- | ---: | --- | --- |
| Alibaba Cloud | `qwen3-asr-flash` | 0.000035 USD / second | Speech-to-text | Equivalent to `qwen3-asr-flash-2025-09-08` |
| Alibaba Cloud | `qwen3-asr-flash-2026-02-10` | 0.000035 USD / second | Speech-to-text | Snapshot model |
| Alibaba Cloud | `qwen3-asr-flash-2025-09-08` | 0.000035 USD / second | Speech-to-text | Snapshot model |
| Alibaba Cloud | `qwen3-asr-flash-filetrans` | 0.000035 USD / second | Speech-to-text | Main model |
| Alibaba Cloud | `qwen3-asr-flash-filetrans-2025-11-17` | 0.000035 USD / second | Speech-to-text | Snapshot model |
| Alibaba Cloud | `qwen3-asr-flash-realtime` | 0.000090 USD / second | Speech-to-text | Equivalent to `qwen3-asr-flash-realtime-2025-10-27` |
| Alibaba Cloud | `qwen3-asr-flash-realtime-2026-02-10` | 0.000090 USD / second | Speech-to-text | Snapshot model |
| Alibaba Cloud | `qwen3-asr-flash-realtime-2025-10-27` | 0.000090 USD / second | Speech-to-text | Snapshot model |

### Realtime translation and streaming transcription

| Provider | Model | Input type | Output type | Price | Capability |
| --- | --- | --- | --- | --- | --- |
| OpenAI | `GPT-Realtime-Translate` | Audio | Audio, text | 0.034 USD / minute or 0.00057 USD / second | Realtime translation |
| OpenAI | `GPT-Realtime-Whisper` | Audio, text | Text | 0.017 USD / minute or 0.00028 USD / second | Realtime streaming speech-to-text |

## Other Models

| Type | Provider | Model | Input price | Output price | Notes |
| --- | --- | --- | ---: | ---: | --- |
| GUI interaction | Alibaba Cloud | `gui-plus-2026-02-26` | 0.21 USD / million tokens | 0.64 USD / million tokens | Converted from RMB pricing by dividing by 7 because overseas pricing was not available. |
| Character roleplay | Alibaba Cloud | `qwen-flash-character-2026-02-26` | 0.05 USD / million tokens | 0.40 USD / million tokens | Each 1M token free quota is valid for 90 days. |
| Music generation | Suno | `suno-v3` | 0.30 USD / call | / | The source notes Suno does not provide an official API platform; price is referenced from api4gpt.com. |
| Music generation | Suno | `suno-v3.5` | 0.30 USD / call | / | The source notes Suno does not provide an official API platform; price is referenced from api4gpt.com. |

## Pricing Tips

1. For language models, confirm context-length tiers and cache behavior before estimating cost.
2. For video models, check resolution, duration, audio mode, whether video input is used, and whether the request is generation or editing.
3. For image models, verify whether the provider charges by image, by tokens, or both.
4. For realtime and audio models, check whether the unit is per million tokens, per second, per minute, or per 10k characters.
5. Free quotas, discounts, and converted prices are provider-specific and should be treated as estimates unless confirmed in the current provider console.

Next, read [Authentication](/en/getting-started/authentication) and [First Request Example](/en/getting-started/first-request) to make your first call.
