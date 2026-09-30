---
title: Browser Requests
definition: Browser requests are calls to luv13 made from JavaScript running in a web page, which luv13 blocks for other sites' origins, so they should go through your own server.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- luv13 rejects cross-origin browser calls from other sites. On 2026-09-30, the CORS preflight for `POST /v1/chat/completions` returned HTTP 400 "Disallowed CORS origin" for `https://example.com` and `http://localhost:3000`.
- So `fetch()` from your own web page to `https://api.luv13.ai/v1/chat/completions` fails in the browser, even with a valid key.
- That's also the safe design: a key in client-side code can be read by anyone who loads the page. luv13.ai/docs says never to put it there.
- Call luv13 from your server, and have your web page call your server.

## Why the browser call fails

A browser sends a preflight `OPTIONS` request before any cross-site request that carries an `Authorization` header. luv13 only approves that preflight for origins it allows. For other origins it answers 400, and the browser never sends the real request. The browser console shows a CORS error.

Check it yourself:

```bash
curl -s -o /dev/null -w "%{http_code}\n" -X OPTIONS \
  https://api.luv13.ai/v1/chat/completions \
  -H "Origin: https://example.com" \
  -H "Access-Control-Request-Method: POST" \
  -H "Access-Control-Request-Headers: authorization,content-type"
```

This printed `400` on 2026-09-30.

## The fix: a small server route

Your page calls your server; your server adds the key and calls luv13. A minimal Node.js 18+ server with no dependencies:

```js
// server.mjs — run with: LUV13_API_KEY=sk-luv13-... node server.mjs
import http from "node:http";

http.createServer(async (req, res) => {
  if (req.method !== "POST" || req.url !== "/api/chat") {
    res.writeHead(404).end();
    return;
  }
  let body = "";
  for await (const chunk of req) body += chunk;
  const { message } = JSON.parse(body);

  const upstream = await fetch("https://api.luv13.ai/v1/chat/completions", {
    method: "POST",
    headers: {
      Authorization: `Bearer ${process.env.LUV13_API_KEY}`,
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      model: "luv13/glm-5.3-flash",
      messages: [{ role: "user", content: String(message) }],
    }),
  });
  res.writeHead(upstream.status, { "Content-Type": "application/json" });
  res.end(await upstream.text());
}).listen(3000);
```

The server fixes the model, so visitors can't pick what runs on your balance. In production, also add your own login or rate limit, since anyone who can reach `/api/chat` spends your credit.

## Related

- [API Key Best Practices](/docs/a/api-key-best-practices)
- [JavaScript Example](/docs/j/javascript-example)
