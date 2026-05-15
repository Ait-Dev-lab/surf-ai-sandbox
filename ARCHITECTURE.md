# Architecture — Surf AI Sandbox

## Overview

The sandbox is the security boundary between the Surf AI browser extension
and the AI backend. It runs in an iframe and communicates with the extension
via `postMessage`. It never exposes AI endpoints, API keys, or model IDs.

```

Extension (postMessage) → Sandbox (iframe)

```

## Layers

### 1. Extension → Sandbox (postMessage)

The extension sends commands to the sandbox via the postMessage protocol.
The sandbox validates the message origin against `ALLOWED_ORIGINS`.

### 2. Sandbox → Communication (REST + Socket.IO)

Authenticated requests are forwarded with:
- `Authorization: Bearer <JWT>`
- `x-session-token: <session_token>`
- `Origin: https://sb-sf.vercel.app`

The sandbox performs no AI processing. It handles:
- **VAD**: Energy-based voice activity detection (vad.js)
- **Audio**: WebM-to-WAV conversion (audio-utils.js)
- **TTS**: Queued playback of AI audio responses (tts.js)


## Security Model

| Layer | Protection |
|-------|-----------|
| Origin validation | Only messages from allowed origins are processed |
| Session tokens | Required for all requests |
| CSP headers | Restrict script sources to sandbox origin |
| No secrets | Zero API keys, tokens, or model IDs in sandbox code |
| No AI processing | All ML inference happens beyond the Sandbox |

## File Map

| File | Purpose |
|------|---------|
| sandbox-main.js | Orchestrator — mic, VAD, mode switching |
| messages.js | postMessage bridge to extension |
| rest-client.js | Communication protocol |
| vad.js | Energy-based voice activity detection |
| tts.js | Audio playback queue |
| audio-utils.js | WebM-to-WAV conversion |
| config.js | Generated at build time from Vercel env vars |
```