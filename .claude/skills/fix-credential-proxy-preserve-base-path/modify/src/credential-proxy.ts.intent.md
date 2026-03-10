# Intent: Preserve base URL path prefix in credential proxy

The credential proxy was discarding the path prefix from `ANTHROPIC_BASE_URL`,
causing 404 errors when using providers like MiniMax that require a prefix
(e.g., `/anthropic/v1/messages` instead of `/v1/messages`).

## Changes

1. Extract the path from the upstream URL after parsing:
   ```typescript
   const basePath = upstreamUrl.pathname.replace(/\/$/, '');
   ```

2. Prepend the base path to all upstream requests:
   ```typescript
   const upstreamPath = basePath + req.url;
   ```

3. Use `upstreamPath` instead of `req.url` in the request options.

## Why

When `ANTHROPIC_BASE_URL` is set to something like `https://api.minimax.chat/anthropic`,
the proxy needs to preserve the `/anthropic` prefix when making upstream requests.
Without this fix, requests would go to `/v1/messages` instead of `/anthropic/v1/messages`.
