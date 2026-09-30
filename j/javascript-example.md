---
title: JavaScript Example
definition: The JavaScript example is a short Node.js script that calls luv13 with the official OpenAI JavaScript SDK.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Install the SDK with `npm install openai` and set `baseURL` to `https://api.luv13.ai/v1`.
- Run it on a server or your own machine, not in a browser. See [Browser Requests](/docs/b/browser-requests).
- Tested on 2026-09-30 with `openai` 7.25.0 on Node.js 20: listing models returned all seven ids, and the chat call without a key failed with status 401.
- For plain `fetch` without the SDK, see [Node.js Fetch](/docs/n/nodejs-fetch).

## The script

Save as `luv13-example.mjs`:

```js
import OpenAI from "openai";

const client = new OpenAI({
  baseURL: "https://api.luv13.ai/v1",
  apiKey: process.env.LUV13_API_KEY,
});

for await (const model of client.models.list()) console.log(model.id);

try {
  const reply = await client.chat.completions.create({
    model: "luv13/glm-5.3-flash",
    messages: [{ role: "user", content: "ping" }],
  });
  console.log(reply.choices[0].message.content);
} catch (err) {
  if (err instanceof OpenAI.APIError) console.error(`HTTP ${err.status}:`, err.error);
  else throw err;
}
```

```bash
npm install openai
export LUV13_API_KEY=sk-luv13-...
node luv13-example.mjs
```

Without a valid key, `err.error` is `{"code":401,"message":"unauthorized","type":"invalid_auth"}`. For the SDKs in general, see [Using the OpenAI SDKs](/docs/u/using-the-openai-sdks).
