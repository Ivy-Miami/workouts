# Setup — do this once, takes about 10–15 minutes

## 1. Create a free database (Supabase)
1. Go to https://supabase.com and sign up (free, no credit card).
2. Click **New project**. Pick any name/password/region, wait ~1 minute for it to spin up.
3. In the left sidebar, click the **SQL Editor**, paste this in, and click **Run**:

```sql
create table logs (
  id bigint generated always as identity primary key,
  date date not null,
  workout text not null,
  exercise text not null,
  set_number int not null,
  target_reps text,
  rir text,
  weight numeric,
  actual_reps numeric,
  done boolean default false,
  notes text,
  updated_at timestamptz default now(),
  unique (date, workout, exercise, set_number)
);

alter table logs disable row level security;
```

This creates the one table the app needs. Row-level security is turned off because
this is a single-user personal tracker with no login — the anon key below can only
reach this one project, not your whole Google/Google Drive account.

## 2. Get your keys
1. In the left sidebar, go to **Project Settings → API**.
2. Copy the **Project URL** (looks like `https://xxxxx.supabase.co`).
3. Copy the **anon public** key (a long string starting with `eyJ...`).

## 3. Paste them into the app
Open `index.html` in any text editor (even Google Docs → "download as .txt" and
rename works, or just edit it on GitHub directly — see step 4) and find these two
lines near the top of the `<script>` section:

```js
const SUPABASE_URL = "PASTE_YOUR_SUPABASE_URL_HERE";
const SUPABASE_ANON_KEY = "PASTE_YOUR_SUPABASE_ANON_KEY_HERE";
```

Replace the placeholder text with your actual URL and key. Save the file.

## 4. Deploy it to a real URL
Easiest option — **Netlify Drop** (no account required for a quick deploy):
1. Go to https://app.netlify.com/drop
2. Drag the whole folder (index.html, manifest.json, sw.js, icon-192.png, icon-512.png) onto the page.
3. It gives you a live URL immediately (e.g. `random-name-123.netlify.app`).
4. (Recommended) Create a free Netlify account afterward so the URL doesn't expire
   and so you can redeploy later if you ever want changes.

Alternative — **GitHub Pages** (a more permanent free option, needs a free GitHub account):
1. Create a new repository on github.com.
2. Upload all 5 files via "Add file → Upload files" in the browser.
3. Go to the repo's **Settings → Pages**, set the source to the `main` branch, save.
4. GitHub gives you a URL like `https://yourname.github.io/repo-name/`.

## 5. Install it on your phone
1. Open your new URL in **Chrome on Android**.
2. Tap **⋮ → Add to Home screen** (or you may see an automatic "Install app" banner).
3. It'll appear as a real app icon and open full-screen, no browser bar.

That's it — from here every entry writes straight to your Supabase database, which is
the same reliable cloud storage Google/Netflix-scale apps use, not the fragile
in-chat storage from before.
