---
name: threejs-webgl-development
description: Create interactive 3D web experiences with Three.js, React Three Fiber, WebGL, shaders, and optimized 3D assets.
---
# Three.js & WebGL Development

## Mission
Build compelling 3D experiences that load quickly, remain interactive and degrade gracefully on constrained devices.

## Workflow
1. Confirm whether the project uses vanilla Three.js, React Three Fiber, Drei or another abstraction.
2. Define the experience and fallback: product viewer, configurator, scroll narrative or interactive scene.
3. Plan scene graph, camera, lighting, materials, controls and responsive behavior.
4. Optimize assets: prefer glTF/GLB, compress geometry and textures, reuse materials and instance repeated objects.
5. Manage renderer lifecycle, resize, device pixel ratio, animation loop and resource disposal.
6. Profile CPU/GPU, draw calls, texture memory, frame time and loading on target devices.
7. Provide reduced-motion, low-power and non-WebGL fallbacks where practical.

## Rules
- Do not create renderers or animation loops repeatedly during React renders.
- Dispose geometries, materials, textures and render targets when no longer used.
- Cap pixel ratio; high resolution multiplies GPU cost.
- Measure before adding expensive shadows, post-processing or shaders.
- Provide accessible equivalent controls and descriptions.
- Treat imported models and shaders as untrusted assets.

## Deliverables
Explain scene architecture, asset pipeline, interactions, performance budget, fallback and device testing.
