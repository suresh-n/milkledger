# The Milk Ledger — Setup Guide

Same architecture as [The Ledger](https://github.com/suresh-n/expense-ledger): a single
`index.html` on GitHub Pages talking to a Google Sheet through Apps Script. No build step,
no server of your own to run.

## 1. Put this on GitHub Pages

1. Create a new GitHub repo (e.g. `milk-ledger`).
2. Upload `index.html` to the repo root (must be named exactly `index.html` for Pages to serve it at the root URL).
3. Go to **Settings → Pages** in the repo.
4. Under "Build and deployment", set **Source: Deploy from a branch**, branch `main`, folder `/ (root)`. Save.
5. GitHub gives you a URL like `https://<your-username>.github.io/milk-ledger/` within a minute or two — that's your app, open it on any device.

## 2. Set up the Google Sheet + Apps Script backend

1. Go to [sheets.google.com](https://sheets.google.com) and create a new blank Sheet. Name it whatever you like (e.g. "Milk Data").
2. In the Sheet, go to **Extensions → Apps Script**.
3. Delete any starter code in `Code.gs` and paste in the contents of `Code.gs` from this folder.
4. Click **Deploy → New deployment**.
5. Click the gear icon next to "Select type" → choose **Web app**.
6. Set:
   - **Execute as:** Me
   - **Who has access:** Anyone
7. Click **Deploy**. Authorize the permissions it asks for (it needs to read/write the Sheet).
8. Copy the **Web app URL** it gives you (looks like `https://script.google.com/macros/s/AKfycb.../exec`).

## 3. Connect the app to the Sheet

1. Open your GitHub Pages URL.
2. Go to the **Settings** tab inside the app.
3. Paste the Web app URL into the "Apps Script Web App URL" field, click **Save URL**.
4. While you're there, set your **default morning/evening quantities** and your **price per liter** — these are saved locally in the browser and drive the Dashboard's bill calculation.
5. Add a test entry from the **Entry** tab, then check the **Ledger** tab shows it, and open the Sheet directly to confirm the row landed there too.

## How it works day to day

- Each day you log **Morning (ml)** and **Evening (ml)** — the form pre-fills with your usual quantities from Settings, so most days it's a single tap.
- The **Ledger** tab shows every day's entry with a running total, straight from the Sheet.
- The **Dashboard** tab replicates the month-end math you were doing by hand in Excel: total ml → quantity in liters → × price per liter → amount to pay. Pick any month from the dropdown to see that month's bill, plus a trend of liters and bill amount over time.

## Notes

- The Sheet is the single source of truth. The Ledger and Dashboard tabs always pull fresh data from it, so laptop and mobile both show the same picture.
- If you ever change the Apps Script code, redeploy via **Deploy → Manage deployments → Edit (pencil icon) → New version → Deploy** — this keeps the same URL rather than generating a new one.
- The Sheet URL, your default quantities, and price per liter are all stored only in each browser's local storage — you'll need to set them once per browser/device.
- Price per liter isn't written to the Sheet — if it changes, just update it in Settings; past months already logged aren't affected retroactively.

## Price history (Prices tab)

Price per liter is stored in a **Prices** tab (columns `from`, `price`) that the script creates automatically the first time you add a price in Settings. Every device reads it from there, so a new machine only needs the Web App URL.

If you update `Code.gs`, redeploy via **Deploy → Manage deployments → Edit (pencil) → New version → Deploy** so the same URL picks up the change.
