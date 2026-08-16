# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

This repo holds **only the built output** (`vite build` → `dist/`) of 生物掃描實驗室 · BioScan Lab — an offline-capable PWA exhibit about Hong Kong biodiversity for kindergarten (K3) children, served free on GitHub Pages (hence `.nojekyll` and the public repo).

**The source code is NOT here.** It lives in a separate local repo at `gravity-park-live-timing-centre/beetle-scanner-exhibit/` (a Vite + TypeScript project). That repo is kept private because it also contains real student data (`data/students.csv`, `data/results.csv`) for a school event, and GitHub Pages requires a public repo — so only the built site is published here.

## Critical rules

- **Do not hand-edit deployed files** (`index.html`, `sw.js`, anything under `assets/`, `content/`, `vendor/`). The next deploy overwrites this repo with a fresh `dist/`, so hand edits are lost — and worse, they desync the service worker (see below). The README states this explicitly.
- **The deploy workflow is**: run `npm run build` in the source repo → copy the `dist/` contents over this repo → commit → push. Feature/code changes can only be made in the source repo; if asked to change app behavior here, explain this and point to the source repo.
- **Service worker precache gotcha**: `sw.js` is a Workbox precache manifest with an MD5 `revision` for every `content/*.json`, `index.html`, and icon (hashed `assets/*` files carry `revision: null` because the hash is in the filename). Editing e.g. `content/catalog.json` directly without regenerating `sw.js` means installed/offline clients keep serving the stale cached copy forever. This is the main reason hand edits here are forbidden — they appear to work in a fresh browser but never reach the exhibit iPads.
- Files editable directly in this repo are limited to repo-level docs that are not part of `dist/`: `README.md` and this `CLAUDE.md`.

## Commands

There is no `package.json`, build, lint, or test tooling in this repo — it is static files plus git.

- Preview locally: any static server from the repo root, e.g. `python3 -m http.server 8080`, then open `http://localhost:8080`. Must be served over HTTP (not `file://`) for the module scripts, JSON fetches, and WASM decoders to load.
- When testing, remember the service worker aggressively caches everything: use a private window or DevTools → Application → "Clear site data" between checks, or you will see stale content.
- `main` is the live GitHub Pages site. Pushing to `main` is deploying.

## Architecture

Single-page app: `index.html` loads one hashed module bundle (`assets/index-*.js`) into `#root`. Everything below is data/assets the bundle fetches at runtime:

- **`content/catalog.json`** — the species registry driving the whole exhibit. Each entry has: `id`, `order`, `emoji`, `view` (`"3d"` → `model` .glb path, or `"2d"` → `illustration` .svg path), `anatomy` (links to the per-species content folder), localized `name`/`tagline`, `sighting` (`common` / `sometimes` / `not-here` — the Hong Kong "how often do you see it" classification), `scene` (`woodland` / `garden` / `water` / `mudflat`), `rotateSpeed`, `credit` (license attribution, required for every asset), optional `photos[]` (real wildlife photos with per-photo credit and localized captions), and optional `hidden: true` (species kept in the data but not shown — currently the 2D-illustration species, since the exhibit now shows only real 3D scans).
- **`content/<species-id>/{yue-Hant-HK,zh-Hant,en}.json`** — per-species, per-locale content: UI strings, anatomy layer labels, and a `tasks[]` sequence (id, title, hint, message, subtitle) that scripts the guided scanning activity, including narration subtitles. Cantonese (`yue-Hant-HK`) is the primary locale (`<html lang>`, manifest, commit messages); written Traditional Chinese and English are the other two. Every localized string exists in all three files.
- **`assets/models/*.glb`** — glTF-Binary scans, Y-up, metric, subject facing +Z. Loaders support Draco, Meshopt, and KTX2/Basis compression; the decoders are vendored offline in `vendor/draco/` and `vendor/basis/` (no CDN — the exhibit must work with no network). The anatomy system (X-ray / layers / exploded views) classifies meshes **by name prefix** (`shell_*`, `hindwing_*`, `muscle_*`, `trachea*`, `digestive*`, `brain`/`nerve*` — see `assets/models/README.md` for the full table; registry lives at `src/modules/anatomy/anatomyRegistry.ts` in the source repo). Unmatched meshes are treated as shell; missing structures show a "not provided" notice instead of breaking. Missing/corrupt models fall back to a placeholder plus a recoverable diagnostic — never a white screen.
- **`assets/audio/`** — narration audio, one folder per locale, one file per task id. **Deliberately empty**: the project refuses to fake human recordings. Dev mode may use `SpeechSynthesisUtterance` (`zh-HK`) for timing; production shows subtitles only and logs a missing-asset diagnostic until licensed pre-recorded Cantonese narration is supplied.
- **PWA layer** — `manifest.webmanifest` (fullscreen, landscape, Cantonese) plus `sw.js`/`workbox-*.js` precaching every asset so the exhibit runs fully offline once installed (intended use: iPad "Add to Home Screen").

## Conventions

- Commit messages are written in Cantonese/Traditional Chinese, describing the exhibit change (e.g. species added, scans swapped in) — follow that style.
- All 3D scans and photos are CC0 / CC BY / CC BY-SA; every asset must carry its attribution in the `credit` fields of `catalog.json`. Full provenance is tracked in `PHOTO_CREDITS.md` in the source repo.
- Honesty-over-polish is a design principle throughout: no fabricated narration audio, no pretending a demo model is a real scan (sample models are labelled 構造示範模型), and graceful degradation with visible diagnostics instead of silent failure.
