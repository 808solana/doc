---
title: JavaScript Example
definition: The JavaScript example is a complete Node.js script that calls luv13 with the official OpenAI JavaScript SDK, from listing models to handling errors.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Install the OpenAI SDK with `npm install openai` and set `baseURL` to `https://api.luv13.ai/v1`.
- Run it on a server or your own machine with Node.js. Don't ship your key to a browser; see [Browser Requests](/docs/b/browser-requests).
- The script below was run on 2026-09-30 with `openai` 7.25.0 on Node.js 20: listing models returned all seven ids, and the chat call failed with status 401 as expected without a key.
- For how the SDKs work with OpenAI-compatible APIs in general, see [Using the OpenAI SDKs](/docs/u/using-the-openai-sdks).

## The script

Save as `luv13-example.mjs`:

```js
import OpenAI from "openai";

const client = new OpenAI({
  baseURL: "https://api.luv13.ai/v1",
  apiKey: process.env.LUV13_API_KEY,
});

// 1. List the models you can use.
for await (const model of client.models.list()) {
  console.log(model.id);
}

// 2. Send a message.
try {
  const reply = await client.chat.completions.create({
    model: "luv13/glm-5.3-flash",
    messages: [{ role: "user", content: "Say hello in five words." }],
  });
  console.log(reply.choices[0].message.content);

  // 3. What it cost: a flat $0.33 per 1M tokens, input = output.
  if (reply.usage) {
    const usd = (reply.usage.total_tokens * 0.33) / 1_000_000;
    console.log(`${reply.usage.total_tokens} tokens = $${usd.toFixed(6)}`);
  }
} catch (err) {
  if (err instanceof OpenAI.APIError) {
    console.error(`luv13 returned HTTP ${err.status}:`, err.error);
  } else {
    throw err;
  }
}
```

Run it:

```bash
npm install openai
export LUV13_API_KEY=sk-luv13-...
node luv13-example.mjs
```

## Notes

- Without a valid key, `err.status` is `401` and `err.error` is `{"code":401,"message":"unauthorized","type":"invalid_auth"}`.
- The SDK retries 429 and 5xx responses on its own. Pass `maxRetries` to the constructor to change that. See [Retrying Requests](/docs/r/retrying-requests).
- To stream, add `stream: true` and use `for await` over the result. See [Streaming on luv13](/docs/s/streaming-on-luv13).

<!-- TODO: run step 2 with a real key and confirm luv13 returns usage in the response. -->
