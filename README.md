# 🩺 Vital Signs — AI-Powered Healthcare Diagnosis Assistant

A free, static, client-side symptom guide. No backend, no API keys, no
database, no ongoing hosting cost. Everything — symptom matching, risk
scoring, history, even "training" on your own data — runs entirely in
the visitor's browser.

> **This is an educational tool, not a medical device.** It does not
> diagnose anyone. See [Limitations](#️-limitations--disclaimer) below.

---

## 🔗 Live Demo

| Platform | URL | Notes |
|---|---|---|
| **Render** | **[ai-powered-healthcare-diagnosis-assistant.onrender.com](https://ai-powered-healthcare-diagnosis-assistant.onrender.com/)** | ✅ Live |
| **GitHub Pages** | **[shristi502102.github.io/AI-Powered_Healthcare-Diagnosis-Assistant](https://shristi502102.github.io/AI-Powered_Healthcare-Diagnosis-Assistant/)** | ✅ Live, never sleeps |

Both serve the exact same `index.html` — pick whichever link is
convenient. GitHub Pages is the more reliable long-term link since free
Render static sites can idle after inactivity; GitHub Pages never does.

---

## 📸 Screenshots

<!--
  Add your own screenshots here — takes under a minute:
  1. Open the live site, press F12 → toggle device toolbar off for desktop shots
  2. Take a screenshot of each screen below (Win: Win+Shift+S, Mac: Cmd+Shift+4)
  3. Save each into a "screenshots" folder in this repo, using the filenames below
  4. Upload that folder to GitHub the same way you uploaded index.html
  Once the files exist at those paths, the images below will render automatically.
-->

| Home / Hero | Symptom Checklist |
|---|---|
| ![Hero section](https://github.com/Shristi502102/AI-Powered_Healthcare-Diagnosis-Assistant/blob/main/image/Screenshot%202026-07-18%20130524.jpg) | ![Symptom checklist](https://github.com/Shristi502102/AI-Powered_Healthcare-Diagnosis-Assistant/blob/main/image/Screenshot%202026-07-18%20131058.jpg)|

| Results — Risk & Matches | Emergency Banner |
|---|---|
| ![Results screen](https://github.com/Shristi502102/AI-Powered_Healthcare-Diagnosis-Assistant/blob/main/image/Screenshot%202026-07-18%20131433.jpg) | ![Emergency banner](https://github.com/Shristi502102/AI-Powered_Healthcare-Diagnosis-Assistant/blob/main/image/Screenshot%202026-07-18%20132242.jpg) |

| Train From Data | AI Explain (optional) |
|---|---|
| ![Train from data](https://github.com/Shristi502102/AI-Powered_Healthcare-Diagnosis-Assistant/blob/main/image/Screenshot%202026-07-18%20133632.jpg)) | ![AI explain](screenshots/ai-explain.png) |

| History Tab | Printable PDF Report |
|---|---|
| ![History](https://github.com/Shristi502102/AI-Powered_Healthcare-Diagnosis-Assistant/blob/main/image/Screenshot%202026-07-18%20133302.jpg) | ![Print report](https://github.com/Shristi502102/AI-Powered_Healthcare-Diagnosis-Assistant/blob/main/image/Screenshot%202026-07-18%20134531.jpg) |

*(Until you add image files, GitHub will just show broken-image icons
here — that's expected and harmless.)*

---

## ✨ Features

- **Symptom checker** — organized checklist across General, Respiratory,
  Digestive, Neurological, Skin, and Other categories
- **Ranked condition matches** — percentage match based on symptom
  overlap against an 18-condition reference table, not a black-box score
- **Risk engine** — combines age, temperature, symptom burden, duration,
  and existing conditions (diabetes, heart disease, immunocompromised,
  pregnancy) into a Low / Medium / High read
- **Emergency detection** — flags dangerous symptom combinations (e.g.
  chest pain + shortness of breath) with an urgent red banner
- **Specialist + precaution guidance** — tells you who to see and what
  to do, per matched condition
- **Printable PDF report** — includes patient name, date, age, symptoms,
  risk level, and matches, formatted cleanly for "Save as PDF"
- **History** — past checks saved to `localStorage` on your device only
- **Train from your own data** — upload a Kaggle-style symptoms→disease
  CSV/XLSX and the app learns new symptom-weight patterns from it (see
  below)
- **AI Explain (optional)** — bring your own Anthropic API key to get a
  plain-language explanation of your results, layered on top of (never
  replacing) the rule-based output

---

## 🧠 How the matching actually works

This is **not** a neural network or a hosted ML model — it's
transparent, auditable logic that runs client-side:

1. Each of the 18 built-in conditions has a small table of symptoms with
   weights (1–3) reflecting how characteristic that symptom is.
2. When you select symptoms, the app computes:
   - **Coverage** — how much of that condition's known pattern you matched
   - **Precision** — how much of *your* selected symptoms that condition explains
   - **Confidence %** — a blend of the two, capped at 97% (never 100%,
     on purpose — see [Limitations](#️-limitations--disclaimer))
3. Conditions are ranked by confidence and the top matches are shown.
4. Risk score is computed separately from age, temperature, symptom
   count, duration, and comorbidities.
5. Emergency rules are checked independently — specific symptom
   *combinations* (not single symptoms) trigger the urgent banner.

### Training from your own dataset

The "Train from data" tab accepts the common Kaggle
"symptoms → disease" shape: one row per patient case, one `0`/`1`
column per symptom, and a `Disease` (or `prognosis`/`diagnosis`) column.

For each disease, it counts how often each symptom appears in that
disease's rows and converts that frequency into a match weight — the
same statistical idea behind a Naive Bayes classifier's priors, done
with a simple `reduce` over the rows. It's genuinely learning from your
data, just not a deep model. Newly discovered symptom columns are
automatically added to the checklist UI. Everything is processed with
[SheetJS](https://sheetjs.com/) in-browser; no file is ever uploaded to
a server, because there is no server.

---

## 🛠️ Tech stack

- **Frontend only:** HTML, CSS, vanilla JavaScript — zero frameworks,
  zero build step
- **Fonts:** Fraunces (headings), Inter (body), IBM Plex Mono (data/numbers)
- **Data parsing:** SheetJS (`xlsx`) via CDN, for the training feature
- **Storage:** `localStorage` for history, saved API key, and trained
  dataset — nothing leaves the browser unless you explicitly enable AI
  Explain
- **Optional AI:** direct browser calls to the Anthropic API, using a
  visitor-provided key (never bundled into the code)

No database. No backend framework. No `.pkl` model files. This was a
deliberate simplification from the original multi-module architecture
sketched in the project's planning doc, in favor of something that
deploys in one click and costs nothing to run.

---

## 📁 Project structure

```
AI-Powered_Healthcare-Diagnosis-Assistant/
├── index.html      → the entire app: markup, styles, logic, dataset
├── README.md        → this file
├── package.json      → tells Railway/Render how to serve it (optional)
├── Procfile      → fallback start command for Railway (optional)
└── screenshots/     → images referenced in this README (add your own)
```

`index.html` is the only file required for the site to work. Everything
else is optional tooling for specific hosts.

---

## 🚀 Deployment

### GitHub Pages (recommended — free, always-on)
1. Push/upload files to your repo
2. **Settings → Pages → Source: Deploy from a branch**
3. Branch: `main`, folder: `/ (root)` → **Save**
4. Live at `https://<username>.github.io/<repo>/](https://ai-powered-healthcare-diagnosis-assistant.onrender.com/` in ~1 minute

### Render (currently also live)
1. [render.com](https://render.com) → sign in with GitHub → **New → Static Site**
2. Connect this repo
3. Build Command: *(leave blank)* · Publish Directory: `.`
4. **Create Static Site**

Note: free Render static sites can idle after inactivity, causing a
slower first load after a period with no visitors. GitHub Pages doesn't
have this behavior.

### Railway (optional, also supported)
`package.json` and `Procfile` are already set up so Railway auto-detects
this as a Node app and serves it with the `serve` package — no config
needed. Deploy from GitHub repo → Railway builds automatically →
**Generate Domain** for a public URL.

---

## 🔒 Privacy

Everything runs client-side. There is no server collecting or storing
patient data. History is saved only in that browser's local storage.
If you enable AI Explain, your typed-in API key is stored only in that
browser and calls go directly from your browser to Anthropic's API —
never through any server belonging to this project.

---

## ⚠️ Limitations & Disclaimer

- **No symptom checker can be 100% accurate**, including this one.
  Confidence scores are capped below 100% deliberately, because
  certainty without a clinical exam and tests isn't honest.
- This is **rule-based pattern matching** against a small reference
  table — not a trained diagnostic model, and it has no knowledge of
  your full medical history.
- It is **not a substitute for professional medical advice, diagnosis,
  or treatment.** Always consult a qualified healthcare provider.
- In an emergency, contact your local emergency number immediately —
  don't wait on any app, including this one.

---

## 🗺️ Possible future additions

- Multi-language support (the original project sheet called out Hindi)
- Voice-based symptom input
- Larger, community-contributed symptom/disease dataset
- Downloadable structured PDF (beyond browser print-to-PDF)

---

## 📄 License

Educational project. Not a licensed medical device. Use, fork, and
modify freely for learning purposes.
