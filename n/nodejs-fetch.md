---
title: Node.js Fetch
definition: Node.js has a built-in fetch function that can call luv13's OpenAI-compatible API with no extra packages.
description: Call luv13 from Node.js with the built-in fetch, including error checks, a request timeout and a runnable script.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- `fetch` is built into Node.js 18 and later, so there's nothing to install.
- A chat call is one `POST` to `https://api.luv13.ai/v1/chat/completions` with a JSON body.
- Put your key in an `Authorization: Bearer` header, read from `process.env`.
- `fetch` doesn't throw on HTTP errors. Check `res.ok` yourself.
- Use `AbortSignal.timeout()` to stop a request that takes too long.

## Example

Save this as `chat.mjs` and run `node chat.mjs`.

```js
const res = await fetch("https://api.luv13.ai/v1/chat/completions", {
  method: "POST",
  headers: {
    "Authorization": `Bearer ${process.env.LUV13_API_KEY}`,
    "Content-Type": "application/json",
  },
  body: JSON.stringify({
    model: "luv13/glm-5.3-flash",
    messages: [{ role: "user", content: "Name one planet." }],
  }),
  signal: AbortSignal.timeout(60_000),
});

if (!res.ok) {
  throw new Error(`luv13 returned ${res.status}: ${await res.text()}`);
}
const data = await res.json();
console.log(data.choices[0].message.content);
```

## Notes

- Keep this on the server. Calling luv13 from browser code would expose your key to anyone who opens the page. See [API Key Best Practices](/docs/a/api-key-best-practices).
- For retries, types and streaming helpers, the [OpenAI SDKs](/docs/u/using-the-openai-sdks) may be easier.
- For reading a streamed reply, see [Server-Sent Events](/docs/s/server-sent-events).

## Related

For a short luv13-only version of this example, see [JavaScript Example](/docs/j/javascript-example).
