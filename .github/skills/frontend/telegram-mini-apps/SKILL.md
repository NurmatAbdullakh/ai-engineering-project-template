---
name: telegram-mini-apps
description: Build Telegram Mini Apps using the Telegram WebApp platform and SDK. Use for web products embedded in Telegram chats, bots, and launch flows.
---
# Telegram Mini Apps

## Mission
Build secure, fast, mobile-first Telegram Mini Apps while treating Telegram as a host platform, not a trusted identity provider by itself.

## Workflow
1. Inspect official Telegram Web Apps documentation and the SDK/version used by the project.
2. Identify launch context: bot menu, inline button, direct link or other supported entry point.
3. Initialize the WebApp bridge safely; handle readiness, theme parameters, viewport, safe areas, BackButton and MainButton as appropriate.
4. Send initData to the backend and validate its signature and auth date server-side. Never trust initDataUnsafe for authorization.
5. Design mobile-first screens that respect Telegram theme colors and platform navigation.
6. Handle unsupported browser contexts and define local development strategy.
7. Test on real Telegram clients and relevant iOS, Android and Desktop variants.

## Rules
- Keep bot tokens and validation secrets on the server.
- Do not treat Telegram user ID alone as proof of a valid session.
- Handle expired authentication data according to backend policy.
- Make payments and sensitive actions server-authoritative.

## Deliverables
Document launch assumptions, bridge integration, backend validation contract, compatibility, tests and deployment requirements.
