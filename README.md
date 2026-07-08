# 15 Pakaraka Drive — Expense Tracker

A single-page web app for tracking shared household and personal expenses between two people. Paste in bank transactions, let Claude auto-categorise them, and get a running dashboard of house contributions, personal spending, savings, investments, and who owes who.

Built as one self-contained `index.html` — no build step, no framework, no server code. Data is stored in Firebase (Auth + Firestore); AI categorisation and invoice reading are done via the Anthropic API.

## Features

- **Dashboard** — House and Personal views with contribution splits, spending trends, savings/investment charts (Chart.js), and a monthly breakdown by category.
- **Categorise** — Paste raw bank statement text, generate a prompt for Claude, paste back the JSON response, then review/adjust the auto-assigned category and sheet (House / Personal / Work / Skip) before saving.
- **History** — Browse and edit past months of categorised transactions.
- **IOU** — Reconciliation between the two users for shared utilities, groceries, and insurance, with a "mark as paid" workflow and a running settle-up summary per month.
- **Invoice analysis** — Drop in an electricity or water invoice and Claude extracts the usage figures automatically.
- **Formulae reference** — In-app documentation of exactly how each dashboard number (house total, personal spend, surplus, etc.) is calculated.
- **Settings** — Manage categories per sheet (House/Personal/Work) and per user.

## Tech stack

- Vanilla HTML/CSS/JS (single file, ES modules)
- [Firebase](https://firebase.google.com/) — Auth (email/password) + Firestore for persistence
- [Chart.js](https://www.chartjs.org/) — dashboard charts
- [Anthropic API](https://docs.anthropic.com/) (Claude) — bank statement categorisation and invoice OCR

## Running locally

This is a static file — no build or install required. Serve it with any static file server, e.g.:

```bash
npx serve .
# or
python -m http.server
```

Then open the served URL in a browser. You'll need to sign in with a Firebase Auth account provisioned in the project's Firebase console.

## ⚠️ Security note

`index.html` currently has a live **Anthropic API key hardcoded in plaintext** (search for `ANTHROPIC_API_KEY`), along with the Firebase web config. Because this repo is on GitHub, that Anthropic key is exposed to anyone who views the page source — **treat it as compromised and rotate it in the Anthropic console.**

Before relying on this repo further:
- Move the Anthropic key out of client-side code (e.g. proxy requests through a small backend/Cloud Function that holds the key server-side).
- Firebase web API keys are safe to expose by design — access is controlled by Firestore security rules, not the key itself — but double check your Firestore rules actually restrict reads/writes to authorized users only.
