# GitHub Pages SPA Template

A starter repository for building and operating a static single-page app on GitHub Pages.

## Why this approach?

This template is optimized for static hosting on GitHub Pages. It enforces clear frontend/backend boundaries, provides deterministic deployment paths, and aligns with browser-native capabilities such as IndexedDB, PWAs, and WASM.

What problems this template solves:

1. **Deployment overhead**: GitHub Pages deployment runs automatically from `main`.
2. **Release reproducibility**: tag-based release creation and release-note flow are built in.
3. **Asset cache control**: release flow updates asset version query strings in `index.html`, helping browsers and PWAs detect fresh assets after deployment.
4. **Documentation consistency**: purpose-definition prompt and release-note style guide standardize project communication.
5. **Static-first architecture discipline**: encourages local-first browser patterns and explicit frontend/backend boundaries.
6. **WORA-oriented delivery with PWA support**: core app behavior can be shared across platforms through browser standards, with platform-specific extensions added only when required.

## Portability and extension model

This template is designed for web-first portability: build once against browser standards, then run across desktop and mobile environments through a PWA-capable SPA.

Portability baseline:

- Keep core UI, state, and business logic in web code shared across platforms.
- Use browser-native capabilities (for example IndexedDB, service workers, and manifests) for installable, local-first behavior.
- Prefer static hosting plus external APIs to keep deployment architecture consistent.

Extension model for OS-level capabilities:

- Keep native-only concerns behind optional per-OS helper services.
- Expose a stable HTTP/WebSocket-style contract so the frontend integration surface remains consistent.
- Treat helper services as adapters, allowing the primary application codebase to remain shared.

## What is included

- `release.ps1`: PowerShell release helper that generates a timestamped diff file, updates asset version query strings in `index.html`, and guides release-note generation.
- `.github/workflows/pages.yml`: Deploys the repository contents to GitHub Pages on pushes to `main` (and supports manual runs).
- `.github/workflows/create-release.yml`: Creates a GitHub Release when a tag is pushed, using `RELEASE_NOTES.md` as the release body.
- `RELEASE_NOTES.md`: Release notes file updated during the release workflow.
- `RELEASE_NOTES_STYLE.md`: Style guide used to keep release notes consistent.
- `VANILLA_JS_APPROACH.md`: Practical implementation guide for framework-free SPAs using HTML templates, `ui.js` helpers, and feature modules.
- `.github/prompts/purpose-definition.prompt.md`: Prompt template for defining app purpose and drafting requirements-ready text.
- `.vscode/launch.json`: VS Code launch configuration for running `release.ps1`.

## Quick start

1. Clone this repository.
2. Add your SPA files (for example `index.html`, CSS, and JavaScript assets).
3. Publish from the repository using GitHub Pages.

## Vanilla JavaScript approach

This repository includes a documented vanilla JavaScript pattern for framework-free, static-hosted SPAs.

Core principles:

- Define UI structure with native HTML `<template>` elements.
- Route DOM creation and mounting through `ui.js` helpers.
- Keep feature scripts focused on orchestration, service logic, and state.
- Use browser-native persistence, including IndexedDB, for local-first behavior.

See `VANILLA_JS_APPROACH.md` for project structure, naming conventions, script loading order, and data-flow guidance.

## Release workflow

Run the release script from PowerShell:

```powershell
./release.ps1
```

Optional flags:

- `-NoPush`: create commit/tag locally without pushing.
- `-WhatIfMode`: preview actions without changing files.

After you push a tag, `.github/workflows/create-release.yml` publishes a GitHub Release automatically.

## GitHub Pages deployment

`.github/workflows/pages.yml` deploys the site automatically when changes are pushed to `main`.

## Supported stack patterns

This template is stack-flexible. The following options map cleanly to GitHub Pages static hosting.

1. **Pure HTML + CSS + JavaScript**
	- Best for: lightweight apps, prototypes, and internal tools.
	- Why it fits: no build pipeline required and direct static deployment.
	- Implementation guide: `VANILLA_JS_APPROACH.md`.
	- Example: Sudoku (https://sudoku.ignyos.com/, repo: https://github.com/Ignyos/Sudoku).

2. **TypeScript + Vite (static output)**
	- Best for: maintainable SPAs with typed code and modern tooling.
	- Why it fits: fast local iteration and predictable static build output.

3. **React, Vue, or Svelte SPA (compiled to static assets)**
	- Best for: component-driven UIs with richer state and routing.
	- Why it fits: framework build output is static and deployable via Pages.

4. **PWA-first SPA (for example, Workbox or Vite PWA plugin)**
	- Best for: installable, offline-capable applications.
	- Why it fits: service workers and web app manifests are compatible with static hosting.

5. **WASM + JavaScript UI shell**
	- Best for: compute-heavy browser features (image/audio processing, parsing, simulation, cryptography).
	- Why it fits: GitHub Pages serves `.wasm` assets and JavaScript orchestrates UI/runtime behavior.
	- Common implementations: Rust + wasm-pack, C/C++ + Emscripten, Go WASM, Blazor WebAssembly.

6. **WASM + PWA**
	- Best for: high-performance applications that also require offline support.
	- Why it fits: PWA lifecycle and caching complement WASM performance for app-like delivery.

7. **Static frontend + external backend API**
	- Best for: applications requiring authentication, multi-user data, or server-side integrations.
	- Why it fits: frontend remains statically deployed on Pages while integrating with hosted APIs.

## Use case examples

- **Personal knowledge app**: TypeScript + Vite + IndexedDB for offline-first note storage and search.
- **Media toolkit**: WASM + JavaScript for in-browser conversion, transcoding, or analysis pipelines.
- **Field checklist app**: PWA SPA for intermittent connectivity with local-first synchronization.
- **Dashboard frontend**: React/Vue/Svelte static UI connected to external APIs for live operational data.
- **Interactive simulator**: WASM + PWA for responsive computation with installable delivery.

## License

This project is licensed under The Unlicense. See `LICENSE` for details.

