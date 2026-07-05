# Machinist Validation App — instructions for Claude sessions

This repo IS the live app. GitHub Pages serves `index.html` from `main` at:
https://richarnoldcamco.github.io/machinist-validation-app/

The owner, Richard Arnold, is a **non-programmer**. Explain things in plain English,
avoid jargon, and never ask him to run commands — do the work for him.

## The one rule that matters

**Everything is `index.html`** — a single self-contained HTML file (~4000 lines) with all
CSS and JavaScript inline. Do not split it into multiple files, add build tools, frameworks,
or npm. External dependencies load from CDNs only (SheetJS, supabase-js, pdf.js, Inter font).

## Versioning convention (required on EVERY change)

Near the top of the `<script>` block:

1. Increment `const APP_REV = NN;` by exactly 1.
2. Update `const BUILD_DATE = "YYYY-MM-DD";` to today.
3. Add a `Rev-NN changelog:` entry above the prior history in the same comment block,
   describing the change in plain English (match the existing style).

The footer, browser tab, and download filename all derive from `APP_REV` automatically.

## Publishing

Pushing to `main` publishes the live site (1–2 min delay). Prefer committing directly to
`main` unless the change is risky. Commit message style: short imperative summary.

## Do-not-break list (features with safety implications)

- **Cloud Sync (Supabase)** — Administration panel. Whole-account snapshot upsert to the
  `snapshots` table. The **load-before-first-save guard** (`mv_cloud_synced_once` localStorage
  flag) prevents a fresh device from overwriting a good cloud backup. Never remove or weaken it.
- **Cloud auto-save** (Rev-87) — debounced push ~4s after data writes; guarded by the same flag,
  paused in preview mode and during snapshot application.
- **Offline-copy warning banner** (Rev-88) — shows on the main menu whenever
  `location.hostname` is not `*.github.io`. Keep it.
- **Error-proofing popup** on wildly out-of-tolerance readings (Rev-81/82). Keep it.
- All user data lives in IndexedDB (`insp_demo` DB) in the browser — the repo contains no
  user data. Never add code that clears IndexedDB outside the existing cloud-load flow.

## Context

- Richard's PC sessions keep local working copies named `MACHINIST VALIDATION - REV-NN.html`
  in a OneDrive folder and copy the newest into this repo's `index.html` to publish. If you
  change `index.html` here (e.g. from a phone session), the PC side will pull before its next
  publish — so always follow the versioning convention so revisions stay in order.
- The default edit password inside the app is stored in settings; do not print or change it.
- QA email default: rarnold@camcomfginc.com.
