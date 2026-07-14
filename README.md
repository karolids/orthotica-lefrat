# LE-FRAT™ Fall Risk Assessment | Orthotica Labs

Single-page clinical tool for the Lower Extremity Fall Risk Assessment Tool (LE-FRAT).
Clinicians complete a 10-item assessment; the page scores it and generates compliance
documentation (Standard Written Order, Summary of Medical Necessity).

Live site: https://orthotica-lefrat.netlify.app

## Structure

The entire application is one self-contained file: `index.html` (inline CSS and JS,
no build step, no dependencies).

## Making changes

1. Edit `index.html` on a branch or directly on `main`.
2. Commit and push. Netlify builds and deploys `main` automatically once the site
   is linked to this repository.
3. Verify the deploy at the live URL before telling anyone about the change.

## History

- Deployed manually (drag-and-drop) to Netlify until July 2026.
- First commit in this repository is a byte-exact copy of the production deploy
  from 2026-05-07 (Netlify deploy `69fc8d8b51d21c4d1ed9faaa`,
  SHA-1 `c90fdf8ea060b767b440de916259837b0cb68349`).

## Patient data policy (print-only architecture)

This tool is deliberately **client-side only**. Patient information entered into the
form exists only in the clinician's browser tab and leaves it exclusively via
`window.print()`. As of July 2026 the page makes **zero network requests** after
load: no form submissions, no analytics, no external scripts/fonts/images, no
browser storage. Because no PHI ever reaches the server, the hosting provider
(Netlify) never handles PHI and no Business Associate Agreement is required for
hosting.

Rules that keep it that way — do not break these without a deliberate,
counsel-reviewed architecture decision:

1. No `fetch`/XHR/beacons, no form `action`s, no WebSockets.
2. No third-party scripts of any kind (analytics, error tracking, tag managers,
   CDNs, web fonts).
3. No `localStorage`/`sessionStorage`/IndexedDB for assessment data.
4. No patient data in URLs (no query-string state, no share links).
5. `netlify.toml` ships a Content-Security-Policy that enforces 1 and 2 at the
   browser level (`connect-src 'none'`, `default-src 'none'`; `img-src data:`
   permits only inline-embedded images and fonts (logo, Poppins) — no network fetches).
   Leave it intact; if a change breaks the page under CSP, the change is the
   problem.
6. Never commit patient data (real or test screenshots containing it) to this
   repository.

## Caution

This tool feeds Medicare compliance documentation. Scoring logic changes should be
reviewed carefully before deploy, and the deploy should be spot-checked by completing
a test assessment end to end.
