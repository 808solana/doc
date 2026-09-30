---
title: Vercel AI SDK
definition: The Vercel AI SDK is a TypeScript library for building AI features that can call luv13 through its OpenAI Compatible provider package.
description: Connect the Vercel AI SDK to luv13 with createOpenAICompatible, with a generateText example and notes on useful options.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Install `ai` and `@ai-sdk/openai-compatible`.
- Create a provider with `createOpenAICompatible`, setting `baseURL` to `https://api.luv13.ai/v1` and `apiKey` from `process.env.LUV13_API_KEY`.
- Call models by their luv13 id, such as `luv13("luv13/glm-5.3-flash")`.
- Functions like `generateText` and `streamText` then work as usual.
- The package can also create embedding models, but luv13 doesn't serve embeddings. Use it for chat only.

## Install

```bash
npm install ai @ai-sdk/openai-compatible
```

## Example

```ts
import { createOpenAICompatible } from "@ai-sdk/openai-compatible";
import { generateText } from "ai";

const luv13 = createOpenAICompatible({
  name: "luv13",
  baseURL: "https://api.luv13.ai/v1",
  apiKey: process.env.LUV13_API_KEY,
});

const { text } = await generateText({
  model: luv13("luv13/glm-5.3-flash"),
  prompt: "Give me three names for a hiking app.",
});

console.log(text);
```

## Options worth knowing

From the SDK's docs for the OpenAI Compatible provider:

- `includeUsage: true` asks for token usage in streamed responses. Whether luv13 returns it while streaming is covered on [Streaming on luv13](/docs/s/streaming-on-luv13).
- `headers` adds custom headers to every request.
- `supportsStructuredOutputs` should only be turned on if the provider supports JSON schema output. See [Structured Outputs](/docs/s/structured-outputs).

## Notes

- Run this on the server, such as in an API route. Don't ship your key to the browser. See [API Key Best Practices](/docs/a/api-key-best-practices).
- Only chat models apply to luv13. Its embeddings and other non-chat endpoints return 404. See [Endpoints](/docs/e/endpoints).
