# Scout Volunteer Hours Tracker

A single-file web app (`index.html`) with two tabs:

- **Volunteer Hours** — date, start/stop time, optional category, and a free-text activity description. Calculates duration automatically, shows monthly totals, and generates a plain-text summary you can paste into your company's reporting tool.
- **Expenses** — expense name, what it was for, amount, date, and an optional receipt (photo or file). Shows a monthly total and a paste-ready summary too.

Works on Mac and mobile browsers, no install required. On mobile, tapping the receipt field lets you take a photo directly or choose an existing one.

## 1. Put it on GitHub Pages (free hosting)

1. Go to github.com and create a new repository, e.g. `scout-hours`.
2. Upload `index.html` to the repository (Add file > Upload files).
3. Go to the repo's **Settings > Pages**.
4. Under "Build and deployment", set Source = "Deploy from a branch", Branch = `main`, folder = `/root`. Save.
5. Wait a minute, then your app is live at `https://YOUR-USERNAME.github.io/scout-hours/`.

Open that URL on your Mac and add it to your phone's home screen (Safari: Share > Add to Home Screen) so it feels like an app.

## 2. Basic use (no setup required)

Just start logging entries. Data saves automatically in your browser's local storage. Use **Download Backup (JSON)** any time to save a copy, and **Restore Backup (JSON)** to load it elsewhere. This alone works great on one device.

## 3. Optional: sync across Mac + phone via Google Drive

Since this is a static site with no server, syncing between devices uses your own Google Drive (a small `scout-hours-data.json` file the app creates for itself — it can't see any other files in your Drive). One-time setup:

1. Go to [console.cloud.google.com](https://console.cloud.google.com) and create a new project (any name, e.g. "Scout Hours").
2. In the left menu: **APIs & Services > Library**, search "Google Drive API", click it, click **Enable**.
3. Go to **APIs & Services > OAuth consent screen**.
   - User type: External. Fill in the required fields (app name, your email).
   - Under "Test users", add your own Google email address.
   - Leave publishing status as "Testing" — this is fine for personal use indefinitely and skips Google's verification review.
4. Go to **APIs & Services > Credentials > Create Credentials > OAuth client ID**.
   - Application type: **Web application**.
   - Under "Authorized JavaScript origins", add `https://YOUR-USERNAME.github.io` (just the domain, no path needed).
   - Click Create. Copy the **Client ID** (looks like `xxxx.apps.googleusercontent.com`).
5. Open the app, expand **Backup & Sync > Google Drive sync setup**, paste the Client ID, click **Save Client ID**, then **Sign in with Google**.

Repeat step 5 (pasting the same Client ID and signing in) on your phone's browser. From then on, entries and expenses you add on either device sync to the same Drive file — click **Sync now** if you want to force it, or it syncs automatically after each change. Receipt photos/files are uploaded to Drive too (as their own files, created only by this app), so a receipt added on your phone becomes viewable on your Mac — tap **View** on that expense row and it fetches and caches it.

Note: this uses a simple "most recent change wins" sync — it's built for one person using two devices, not simultaneous editing on both at once.

## 4. Monthly report / export

- The **Monthly Report** card (Volunteer Hours tab) lets you pick a month and shows a running total plus a ready-to-paste text block (date, activity, category, hours per entry, and a grand total) — click **Copy to Clipboard** and paste it into your company's volunteer reporting form.
- The **Monthly Expense Report** card (Expenses tab) does the same for expenses: date, name, amount, purpose, and a grand total.
- **Export CSV** on either tab downloads all records as a spreadsheet file. The expense CSV lists whether each row has a receipt attached, but doesn't embed the receipt image itself — use the JSON backup below for that, or view/save receipts individually from the Expenses table.

## 5. Receipts — how they're stored

Receipt photos/files are stored in your browser (IndexedDB, no size limit like local storage has) and, if Google Drive sync is set up, also uploaded to your Drive. **Download Backup (JSON)** bundles receipt images into the backup file too, so a full backup is self-contained — just bigger if you have a lot of photos.

## Files

- `index.html` — the entire app (HTML/CSS/JS, no build step, no dependencies besides Google's optional sign-in script).
