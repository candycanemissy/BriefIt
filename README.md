# Briefit — live on Claude + Gmail

A mobile-style dashboard that reads your Gmail, runs the Scanner → Extractor →
Briefer pipeline on Claude, and shows one morning briefing. Nothing to paste.

- `index.html` — the app (blue theme). Works offline in **Demo mode**.
- `api/briefit.js` — serverless function: reads Gmail (IMAP) + runs Claude.
- `package.json` — IMAP deps (Vercel installs them automatically).
- `vercel.json` — lets the function run up to 60s.

## Step 1 — Get a Gmail App Password (≈3 min)
Reads the inbox of ONE account (the demo account). No Google Cloud project needed.
1. Go to the Google Account used for the demo → **Security**.
2. Turn on **2-Step Verification** (required for app passwords).
3. Open **App passwords** (https://myaccount.google.com/apppasswords), name it
   "Briefit", and copy the **16-character password** (no spaces).

## Step 2 — Deploy to Vercel
1. Push this folder to a GitHub repo.
2. vercel.com → Add New → Project → Import the repo (no build settings).
3. Settings → **Environment Variables**, add:

   | Name | Value |
   |------|-------|
   | `ANTHROPIC_API_KEY` | your Claude API key (console.anthropic.com) |
   | `GMAIL_USER` | the demo Gmail address, e.g. you@gmail.com |
   | `GMAIL_APP_PASSWORD` | the 16-char app password from Step 1 |
   | `ANTHROPIC_MODEL` | optional — defaults to `claude-haiku-4-5-20251001` |
   | `BRIEFER_MODEL` | optional — defaults to `claude-sonnet-5` |

4. Click **Deploy**. You get a live `https://<your-app>.vercel.app` URL.

## Step 3 — Run the demo
Open the URL on your phone or laptop.
- **Live:** turn **Demo off**, keep "Fetch inbox", pick 10, tap ➜ — it reads the
  real inbox and briefs it.
- **Safe fallback:** leave **Demo on** — runs the built-in sample instantly, no
  network. Use this if the venue Wi-Fi is flaky. (Always rehearse both.)

## Notes
- Only reads the inbox — never sends or deletes anything.
- Classify + extract run on Haiku 4.5, the briefing on Sonnet 5. Structured
  output uses Claude tool-use, so the JSON is schema-guaranteed.
- To read a different account, just change `GMAIL_USER` / `GMAIL_APP_PASSWORD`
  and redeploy.
