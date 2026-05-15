# Surf AI — Sandbox

Secure iframe sandbox for the Surf AI browser extension. Handles authentication, voice activity detection, audio processing, and model routing. Communicates with the extension via postMessage and forwards requests to the Gateway.

## Architecture

```

Extension (postMessage) → Sandbox (iframe) → AI Models

```

The sandbox is the security boundary between the extension and the backend. The extension never touches the AI models directly.

## What It Does

- **Auth**: Receives JWT from extension, manages session tokens
- **Voice**: Energy-based VAD, push-to-talk, real-time voice mode
- **Audio**: WebM-to-WAV conversion in-browser
- **Models**: Friendly model aliases mapped at the Gateway
- **Streaming**: SSE token-by-token responses
- **Security**: ALLOWED_ORIGINS, session tokens, Origin enforcement

## Message Protocol

```js
// Extension → Sandbox
{ type: "extension_command", action: "sendText", text: "Hello", model: "surf" }

// Sandbox → Extension
{ type: "response", text: "Hi!", responseType: "text" }
```

Configuration

All config is generated from Vercel environment variables at build time. Zero hardcoded URLs in the repo.
