---
'@modelcontextprotocol/client': patch
---

Ignore a progress notification that loses the race against its request's response instead of reporting it through `onerror`. A server's terminal `progress === total` frame and its result are adjacent writes, and the client dispatches responses synchronously while notifications are deferred, so the notification can legitimately arrive for a token whose request just completed. Recently completed progress tokens are now remembered in a bounded FIFO (64 entries) and matching notifications are dropped quietly; progress for a token that was never issued still reports the existing `unknown token` error.
