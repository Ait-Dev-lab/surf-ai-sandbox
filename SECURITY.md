# Security Policy

## Reporting a Vulnerability

If you discover a security vulnerability in the Surf AI sandbox, please
do not open a public issue. Instead, Send Message:

**https://surf-ai-site.web.app/contact.html**

Please include:
- A detailed description of the vulnerability
- Steps to reproduce
- Potential impact

We aim to acknowledge reports within 48 hours and provide a fix timeline
within 7 days.

## Security Model

The sandbox is designed to be the security boundary between the browser
extension and the AI backend. Key principles:

1. **The sandbox holds zero secrets.** All API keys, model IDs, and
   provider URLs live in the Gateway's Render environment variables.

2. **The sandbox is fully public.** Source code is intentionally
   visible so anyone can audit it. Security comes from architecture,
   not obscurity.

3. **Defense in depth.** Origin validation, session tokens, JWT
   authentication, and rate limiting all operate independently.

4. **No AI processing.** The sandbox handles audio conversion and VAD
   only. All ML inference happens beyond the G
Sandbox.

## Headers

| Header | Value |
|--------|-------|
| Content-Security-Policy | `default-src 'self' 'unsafe-inline' 'unsafe-eval'; frame-ancestors *; media-src 'self'` |
| X-Content-Type-Options | `nosniff` |
| Referrer-Policy | `strict-origin-when-cross-origin` |
| Permissions-Policy | `microphone=(self), autoplay=(self)` |
| Strict-Transport-Security | `max-age=63072000; includeSubDomains; preload` |

## Scope

This policy covers the sandbox code in this repository and the deployed
sandbox.

The rest of the backend are out of scope
for this repository — they live in private infrastructure.