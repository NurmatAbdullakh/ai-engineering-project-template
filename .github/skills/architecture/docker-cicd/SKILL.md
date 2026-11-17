---
name: docker-cicd
description: Containerize applications and build reliable CI/CD pipelines using Docker, Compose, GitHub Actions, and deployment automation.
---
# Docker & CI/CD

## Mission
Make builds reproducible and releases repeatable, observable and recoverable.

## Workflow
1. Inspect app structure, runtime, environment, current deployment and pipeline.
2. Define build, test, artifact and deployment stages.
3. Use multi-stage Docker builds, pinned dependencies, non-root users and minimal production images.
4. Separate build-time and runtime configuration; keep secrets out of images and logs.
5. Add CI gates: install, lint, typecheck, tests, build and relevant security checks.
6. Deploy immutable artifacts; use health checks, readiness and graceful shutdown.
7. Plan migration ordering, rollback and compatibility between app versions.
8. Verify deployed version and health.

## Rules
- Never commit secrets or embed credentials in Docker layers.
- Do not use latest as the only production version pin.
- Avoid destructive deployment commands without approval.
- Keep staging and production configuration distinct.
- Use least-privilege CI tokens.
- A green CI run does not prove production health.

## Deliverables
Document pipeline stages, image strategy, secrets, deployment sequence, health checks, rollback and verification.
