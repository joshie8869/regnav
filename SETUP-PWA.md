# Reg Navigator — Install as a Real App (iPhone + Android)

Reg Navigator runs on Google Apps Script, which wraps web apps in Google's own page —
so the app can't hand your phone its icon and manifest directly. This shell fixes that:
a tiny static site that carries the ring-dot icon, the manifest, and a service worker,
and runs Reg Navigator full-screen inside it. Host it once, install it everywhere.

---

## Step 1 — One required Apps Script setting

The shell loads your app inside a frame. For that to work reliably on phones
(especially iOS standalone mode, which isolates Google's login cookies):

1. Apps Script → **Deploy → Manage deployments → ✏️ (pencil)**
2. **Who has access: "Anyone"** (Execute as: **Me** — unchanged)
3. Deploy (this is a "New version" on the existing deployment — URL stays the same)

**Honest tradeoff:** "Anyone" means anyone *with the exact URL* can use the app —
the URL is a long unguessable token, it's linked nowhere public, and your API keys,
Script Properties, and log sheet all stay locked inside your Google account. Their
usage would bill your API keys, so don't share the URL. If you're not comfortable
with that, skip the shell and use "Add to Home Screen" on the /exec page directly —
everything works, you just get a generic icon.

## Step 2 — Host the shell on GitHub Pages (free, ~5 minutes)

1. Open `pwa-shell/index.html` and set the one config line near the top:
   `var APP_URL = 'https://script.google.com/macros/s/AKfycb.../exec';`
   (your Reg Navigator exec URL)
2. github.com → **New repository** → name it `regnav` → Public → Create
3. **Add file → Upload files** → drag in the CONTENTS of the `pwa-shell` folder
   (`index.html`, `manifest.webmanifest`, `sw.js`, and the `icons` folder) → Commit
4. Repo **Settings → Pages** → Source: **Deploy from a branch** → Branch: `main`, folder `/ (root)` → Save
5. Wait ~2 minutes → your app lives at `https://<your-username>.github.io/regnav/`

Open that URL on your phone — you should see the spinning ring boot screen, then the app.

## Step 3 — Install

**iPhone (Safari):**
1. Open `https://<your-username>.github.io/regnav/` in **Safari** (must be Safari)
2. Share button (□↑) → **Add to Home Screen** → Add
3. The ring-dot icon lands on your home screen; launches full-screen, no browser chrome

**Android (Chrome):**
1. Open the same URL in Chrome
2. Either tap the app's own **"Install Reg Navigator?"** pill when it appears,
   or ⋮ menu → **Add to Home screen / Install app**
3. Installs as a real app — appears in the app drawer, full-screen launch

## Updating later

- **App changes** (RegNav.html / Code.gs): normal flow — paste, save, Manage deployments →
  New version. The installed app picks it up on next launch automatically. Shell untouched.
- **Shell changes** (rare): edit the file in GitHub, commit. If an icon or manifest change
  doesn't show up, bump `regnav-shell-v1` to `-v2` in `sw.js` and reinstall.

## What you get installed

- Ring-dot icon (dark + maskable variants so Android's icon masks never clip the rings)
- Full-screen standalone launch, dark themed, notch/safe-area aware
- Camera (📷), voice (🎙), and clipboard permissions delegated to the app frame
- Pinch/pan/double-tap zoom on every diagram; friendly offline notice when signal drops
- The reasoning engine itself always runs live — it needs a connection to think
