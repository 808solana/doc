---
title: Image Input
definition: Image input means sending a picture inside a luv13 chat completion message so a model that accepts images can read it and answer in text.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- luv13.ai/models lists image input for five of the seven models: `luv13/kimi-k3`, `luv13/kimi-k3-fast`, `luv13/glm-5.3-flash`, `luv13/deepseek-v4.1-flash` and `luv13/qwen-3.8-27b`.
- `luv13/glm-5.3` and `luv13/deepseek-v4-pro` are listed as text in only.
- Every model returns text only. luv13 doesn't generate images; `POST /v1/images/generations` returns 404.
- In the OpenAI format, an image goes in the message `content` as an `image_url` part, either a public URL or a base64 data URL.
- Images count as input tokens and are billed at the same $0.33 per 1M.

<!-- TODO: verify with a real key that luv13 accepts the image_url format below, which image types and sizes it takes, and how image tokens are counted. -->

## Example with a URL

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [{
      "role": "user",
      "content": [
        {"type": "text", "text": "What is in this picture?"},
        {"type": "image_url", "image_url": {"url": "https://example.com/photo.jpg"}}
      ]
    }]
  }'
```

## Example with a local file

Encode the file as a data URL. This builds the JSON with `jq` so the long string is handled safely:

```bash
IMG=$(base64 -w0 photo.png)
jq -n --arg img "data:image/png;base64,$IMG" '{
  model: "luv13/glm-5.3-flash",
  messages: [{role: "user", content: [
    {type: "text", text: "Describe this image."},
    {type: "image_url", image_url: {url: $img}}
  ]}]
}' | curl -s https://api.luv13.ai/v1/chat/completions \
      -H "Authorization: Bearer $LUV13_API_KEY" \
      -H "Content-Type: application/json" \
      -d @-
```

On macOS, use `base64 -i photo.png` instead of `base64 -w0 photo.png`.

## Video

luv13.ai/models also lists video input for Kimi K3, Kimi K3 Fast, GLM-5.3 Flash and Qwen 3.8 27B. The OpenAI chat format has no standard video part, and luv13 hasn't published how to send video.

<!-- TODO: ask the operator for the request format for video input, and add an example once verified. -->

For each model's input types, see [Models](/docs/models).
