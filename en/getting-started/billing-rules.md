---
title: "Billing Rules"
description: "The five billing rules used by the API aggregation platform: usage-based, per-call, duration-based, resolution-based, and tiered pricing."
---

This page explains the common billing rules used by the API aggregation platform. For concrete model prices, see [Models and Pricing](/en/getting-started/pricing). This page only explains how to read and estimate the billing units.

A model can use more than one rule at the same time. For example, video models are usually billed by duration and also vary by resolution. Text models are usually usage-based and may also use context-length tiers.

## Billing Rule Overview

| Rule | Common units | Typical use cases | Estimate |
| --- | --- | --- | --- |
| Usage-based | Tokens, characters, input volume, output volume | Language models, multimodal models, text-to-speech | Usage × unit price |
| Per-call | Image, request, call | Image generation, music generation, some utility models | Count × per-unit price |
| Duration-based | Second, minute | Video generation, speech-to-text, realtime audio | Duration × duration unit price |
| Resolution-based | 480p, 720p, 1080p, 4K, standard, pro | Video generation, image generation | Choose the resolution, then use its price |
| Tiered | Context length, resolution tier, usage tier | Text models, long-context models, some multimodal models | Find the matching tier, then calculate with that tier's price |

## Usage-Based

Usage-based billing charges by the actual amount of input or output. Common units include tokens, characters, and modality-specific input volume.

Typical cases:
- Text models bill input tokens and output tokens separately.
- Multimodal models may have separate prices for text, image, video, and audio input.
- Text-to-speech commonly bills by input characters.

To estimate cost, first check the unit in the pricing table, such as USD / million tokens or USD / 10k characters, then convert your expected usage into the same unit.

## Per-Call

Per-call billing charges by generated item count or request count.

Typical cases:
- Image generation bills by output image count.
- Some models bill by each completed call.

When estimating, confirm how many billable outputs a request creates. For example, if one image request generates 4 images, it is usually billed as 4 images.

## Duration-Based

Duration-based billing charges by the length of generated or processed audio/video. Common units are seconds and minutes.

Typical cases:
- Video generation bills by output video duration.
- Speech-to-text bills by input audio duration.
- Realtime audio services may bill by minute or second.

When estimating, confirm whether the billable duration is input duration or output duration. Video generation usually uses output duration, while speech-to-text usually uses input audio duration.

## Resolution-Based

Resolution affects unit price. The same model can have different prices for different resolution or quality levels.

Common tiers:
- Video: 480p, 720p, 1080p, 4K.
- Quality: standard, high quality, pro.

Choose the resolution first, then use the corresponding price. Do not estimate high-resolution or 4K requests with lower-resolution prices.

## Tiered

Tiered pricing means the same model uses different prices for different ranges.

Common tiers:
- Context length tiers, such as `[0, 32]k`, `(32, 128]k`, and `(128, 256]k`.
- Resolution tiers, such as 720p and 1080p.
- Input/output combination tiers, where input length and output length together determine the price.

Estimate by first finding the tier your request falls into, then using the input, output, or cache price for that tier.

## Recommendations

1. Choose the model first, then identify which billing rule or rules it uses.
2. For text and multimodal models, check whether input, output, and cache are billed separately.
3. For video models, check duration, resolution, audio mode, and whether the request is generation or editing.
4. For image models, check output image count, resolution, and prompt rewrite settings.
5. For audio models, check whether the unit is character, second, or minute.
6. Free quotas, discounts, and cache rules can change the final cost, so calculate them separately.

Next, see [Models and Pricing](/en/getting-started/pricing) to choose a concrete model, or [First Request Example](/en/getting-started/first-request) to make an API call.
