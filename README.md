# Vital Signs — Symptom Guide

A free, static, client-side symptom checker. No backend, no API keys, no
ongoing cost. Everything (matching logic, risk scoring, history) runs in
the visitor's browser using plain HTML/CSS/JS.

**This is an educational tool, not a medical device.** It cannot and does
not claim diagnostic accuracy — see the in-app "About & limits" tab.

## Run locally

Just open `index.html` in a browser. No build step, no install.

Or serve it locally:
```bash
npx serve .
```

## Deploy on GitHub Pages (free)

1. Push this folder to a GitHub repo.
2. Go to **Settings → Pages**.
3. Under "Build and deployment", set **Source: Deploy from a branch**.
4. Choose branch `main`, folder `/ (root)`, then **Save**.
5. Your site will be live at `https://<username>.github.io/<repo>/` within
   a minute or two.

No workflow file needed — it's a static site.

## Deploy on Railway (free tier)

This repo includes both a `package.json` and a `Procfile` so Railway's
Nixpacks builder detects it automatically as a Node app and serves the
static files with zero configuration:

1. Create a new Railway project → **Deploy from GitHub repo**.
2. Select this repo. Railway auto-detects Node via `package.json`.
3. It will run `npm start`, which serves the folder with the `serve`
   package on Railway's assigned `$PORT`.
4. Once deployed, click **Generate Domain** to get a public URL.

No environment variables, database, or API key required.

## Project structure

```
index.html    → entire app (markup, styles, logic, data)
package.json  → tells Railway how to serve it
Procfile      → fallback start command
README.md     → this file
```

## New features

**Printable PDF report** — the "About you" form now has a Name field. Tap
"Download / print report (PDF)" on the results screen; a clean,
button-free version of the report (name, date, age, symptoms, risk,
matches) is what prints — use your browser's "Save as PDF" destination.

**Train from your data** — the "Train from data" tab accepts a .xlsx or
.csv file in the common Kaggle "symptoms → disease" shape: one row per
case, one 0/1 column per symptom, and a `Disease` (or `prognosis` /
`diagnosis`) column with the label. It counts how often each symptom
co-occurs with each disease in your file and turns that into match
weights, added alongside the built-in dataset. This is simple, transparent
statistical learning — not a neural network — and it all happens in the
browser via SheetJS; no file is uploaded anywhere. It's remembered in
that browser via local storage; use "Reset to built-in dataset" to undo.

**AI explain (optional, bring your own key)** — off by default, costs
nothing unless a visitor turns it on with their own Anthropic API key.
The key is stored only in that browser's local storage and calls
`api.anthropic.com` directly from the browser — there's still no backend.
Anyone who opens dev tools on that browser could see the key, so this is
meant for personal use, not for asking random site visitors to paste in
a key on your behalf. It adds a plain-language explanation under the
rule-based results; it never replaces or overrides the rule-based scores.

## Extending it

- Add more conditions/symptoms by editing the `CONDITIONS` and
  `SYMPTOM_GROUPS` objects near the top of the `<script>` block in
  `index.html`.
- To add an LLM-generated plain-language explanation on top of the
  rule-based match, you'd need a small backend to hold an API key
  (never put an LLM API key in client-side JS). That's an optional
  paid step — the app is fully functional and free without it.

## Disclaimer

Not a substitute for professional medical advice, diagnosis, or
treatment. Always consult a qualified healthcare provider. In an
emergency, contact your local emergency number immediately.
