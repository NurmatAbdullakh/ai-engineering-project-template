---
name: frontend-performance
description: Diagnose and improve frontend loading, rendering, runtime, and Core Web Vitals.
---
# Frontend Performance

## Mission
Measure first, identify bottlenecks and improve user-perceived performance without sacrificing correctness.

## Workflow
1. Establish target devices, network conditions, routes and user journeys.
2. Capture baseline using Lighthouse or equivalent, Web Vitals, bundle analysis and browser Performance/Memory panels.
3. Separate network, server, JavaScript, rendering, layout, image/font and GPU bottlenecks.
4. Prioritize by user impact and define measurable acceptance criteria.
5. Apply targeted fixes: code splitting, lazy loading, asset optimization, caching, reduced hydration or virtualization when justified.
6. Re-measure under the same conditions and report trade-offs.

## Rules
- Track LCP, INP and CLS alongside route-specific metrics.
- Avoid premature memoization and speculative caching.
- Prevent layout shifts with reserved media dimensions and stable UI.
- Optimize images, fonts and third-party scripts.
- For Three.js, profile frame time, draw calls, GPU memory and pixel ratio.
- Respect reduced-motion preferences.

## Deliverables
Include baseline, root cause, changes, before/after evidence and regressions checked.
