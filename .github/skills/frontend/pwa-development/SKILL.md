---
name: pwa-development
description: Create installable Progressive Web Apps with manifests, service workers, offline behavior, and push notifications.
---
# Progressive Web App Development

## Mission
Turn a web application into a reliable installable PWA without compromising normal browser behavior or security.

## Workflow
1. Inspect framework, hosting, HTTPS, existing service worker and caching setup.
2. Separate requirements: installability, offline shell, offline data, push and background sync.
3. Create a valid Web App Manifest with identity, icons, start URL, scope and display mode.
4. Choose caching per resource: precache immutable shell assets; use network-first or stale-while-revalidate for suitable content.
5. Version caches and clean old caches safely on activation.
6. Provide update UX so users understand when a new version is ready.
7. Test install, update, offline, slow network, storage pressure and browser support.

## Security and reliability
- Service workers require secure contexts except localhost.
- Never cache tokens or private API responses by default.
- Treat push payloads as untrusted input.
- Explain platform limitations; browser support differs.

## Deliverables
List manifest and service-worker changes, caching matrix, offline guarantees, update behavior and test evidence.
