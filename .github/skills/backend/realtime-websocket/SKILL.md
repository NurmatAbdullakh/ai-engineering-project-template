---
name: realtime-websocket
description: Design real-time communication with WebSockets, SSE, and polling for live dashboards, POS updates, and notifications.
---
# Real-Time Communication

## Mission
Choose the simplest real-time transport that meets latency, directionality, scale and reliability needs.

## Transport selection
- Polling: simple periodic updates, higher request overhead.
- Long polling: useful where persistent streaming is unavailable.
- SSE: server-to-client event stream over HTTP.
- WebSocket: bidirectional, low-latency persistent communication.

## Workflow
1. Define events, delivery expectations, ordering, authorization and reconnect behavior.
2. Choose transport based on interaction; do not default to WebSocket.
3. Authenticate connections and authorize each subscription/action.
4. Define message envelope, schema version, event ID and correlation ID.
5. Add heartbeat, reconnect with jitter, backpressure, payload limits and rate limits.
6. For multiple instances, use shared pub/sub or a broker; plan sticky sessions only if needed.
7. Recover missed events through replay, cursors or canonical-state refetch.
8. Test disconnects, duplicates, slow consumers and horizontal scaling.

## Rules
- A socket connection does not authorize every channel.
- Do not assume exactly-once delivery; design idempotent consumers.
- Keep durable business events outside transient socket memory.
- Monitor connection count, send queue, latency and reconnect rates.

## Deliverables
Explain transport choice, event contract, auth, reconnect/replay, scaling and failure behavior.
