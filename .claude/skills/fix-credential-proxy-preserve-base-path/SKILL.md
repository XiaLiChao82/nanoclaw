# fix-credential-proxy-preserve-base-path

Fix the credential proxy to preserve the path prefix from `ANTHROPIC_BASE_URL`.

## Problem

When using API providers like MiniMax that require a path prefix in their base URL
(e.g., `https://api.minimax.chat/anthropic`), the credential proxy was discarding
the `/anthropic` prefix, causing 404 errors.

## Solution

This fix preserves the base URL path prefix when proxying requests:

- Extracts the path from `ANTHROPIC_BASE_URL` (e.g., `/anthropic`)
- Prepends this path to all upstream requests
- Ensures requests go to `/anthropic/v1/messages` instead of `/v1/messages`

## Example

Before:
```
ANTHROPIC_BASE_URL=https://api.minimax.chat/anthropic
Request: POST /v1/messages
Upstream: https://api.minimax.chat/v1/messages (404)
```

After:
```
ANTHROPIC_BASE_URL=https://api.minimax.chat/anthropic
Request: POST /v1/messages
Upstream: https://api.minimax.chat/anthropic/v1/messages (200)
```

## Apply

```bash
setup load fix-credential-proxy-preserve-base-path
npm run build
```
