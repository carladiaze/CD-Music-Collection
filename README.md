# 💿 CD Collection Manager

A self-contained, open-source web app for cataloguing your physical CD collection. No backend required to run — just open the HTML file in any browser. Optional Supabase integration for cloud sync across devices.

Built with vanilla HTML, CSS, and JavaScript. Zero dependencies beyond optional Supabase for sync and ZXing for barcode scanning.

---

## Features

- **Search & add** albums via iTunes API (no API key needed)
- **Barcode scanner** — point your camera at a CD barcode to look it up instantly
- **Cover art** — auto-fetched from iTunes, or upload your own / paste a URL
- **Full metadata** — artist, title, year, label, genre, country, catalog number, shelf, copies, tracklist, personal notes
- **Edit everything** — all fields editable, including tracklist line by line
- **Shelf organization** — filter your collection by custom shelf labels
- **Grid and list views** — sort by artist, title, year, or recently added
- **Online search links** — every album links to Amazon, Discogs, Google Images, eBay, MusicBrainz
- **Cloud sync** — optional Supabase backend syncs your collection across all devices
- **Cross-device setup** — one-click setup link to connect new devices without re-entering credentials
- **Copies tracking** — log how many physical copies you own of each title
- **Offline-first** — works fully without internet; syncs when connected

---

## Quick start

### Option A — Local only (no sync)

1. Download `cd-collection.html`
2. Open it in any browser
3. Start adding CDs

Your collection saves to your browser's `localStorage`. It stays on that device and browser only.

### Option B — Cloud sync across devices (recommended)

Follow the Supabase setup below, then deploy to Netlify so you can access it from any device.

---

## Supabase setup (free)

Supabase is a free open-source Firebase alternative. The free tier is more than enough for a personal CD collection.

### 1. Create a project

1. Go to [supabase.com](https://supabase.com) and sign up
2. Click **New project**
3. Give it a name (e.g. "Music Collection")
4. Choose a strong database password — save it somewhere
5. Select the region closest to you
6. Make sure **Enable Data API** and **Enable automatic RLS** are both checked
7. Click **Create new project** and wait ~2 minutes

### 2. Create the collection table

Go to **SQL Editor** in the left sidebar and run:

```sql
create table cd_collection (
  id text primary key,
  data text not null,
  updated_at timestamptz default now()
);

alter table cd_collection enable row level security;

create policy "Allow all"
on cd_collection for all
using (true)
with check (true);
```

### 3. Create the storage bucket for cover images

Go to **Storage** in the left sidebar:

1. Click **New bucket**
2. Name it exactly `covers`
3. Toggle **Public bucket** ON
4. Click **Save**

Then go back to **SQL Editor** and run:

```sql
create policy "Allow public uploads"
on storage.objects for insert
to anon
with check (bucket_id = 'covers');

create policy "Allow public updates"
on storage.objects for update
to anon
using (bucket_id = 'covers');

create policy "Allow public reads"
on storage.objects for select
to anon
using (bucket_id = 'covers');

create policy "Allow public deletes"
on storage.objects for delete
to anon
using (bucket_id = 'covers');
```

### 4. Get your credentials

Go to **Project Settings → API**:

- **Project URL** — looks like `https://abcdefgh.supabase.co`
- **anon / public key** — the long `eyJ...` string under "Project API keys"

> **Security note:** The anon key is designed to be used in client-side apps. It's safe to use here. Never use the `service_role` key in a browser app.

### 5. Connect the app

Open `cd-collection.html`, click **☁ offline** in the top right, paste your URL and anon key, set a password, and click **Connect & Sync**.

---

## Deploying to Netlify (access from any device)

The easiest way to make the app accessible from your phone, tablet, or any other device.

### Netlify Drop (no account needed)

1. Go to [netlify.com/drop](https://app.netlify.com/drop)
2. Drag and drop `cd-collection.html` onto the page
3. You get an instant URL like `https://graceful-torte-abc123.netlify.app`

### With a Netlify account (recommended — keeps your URL permanent)

1. Create a free account at [netlify.com](https://netlify.com)
2. Go to **Sites → Add new site → Deploy manually**
3. Drag and drop the file
4. In **Site configuration → General** you can rename your site to something like `yourname-cds.netlify.app`
5. To update: drag and drop the new file again

> **Note:** The barcode scanner requires HTTPS to access the camera. The Netlify URL provides this automatically. It won't work opening the HTML file directly from your filesystem.

---

## Setting up on a second device

Once you're connected on one device:

1. Click the ☁ sync button
2. Click **🔗 Copy setup link for other devices**
3. Send the link to yourself (email, WhatsApp, etc.)
4. Open it on the other device — it auto-connects

The link encodes your credentials in the `#hash` fragment, which is never sent to any server.

---

## Adding CDs

### By search
Type an artist or album name in the search bar. The app queries iTunes across multiple regional stores (US, UK, Colombia, Mexico, Spain) for the best coverage.

### By barcode
Click **📷 Scan barcode** and point your camera at the CD's barcode. It looks up the exact pressing in MusicBrainz's database — much more accurate than name search.

If camera access isn't available, a manual barcode entry field appears.

### Manually
Click **Add manually** in the sidebar to enter all fields by hand.

---

## Fixing cover art

When the auto-fetched cover is wrong:

1. Open the album
2. Click the search links at the top (**Google Images**, **Discogs**, **Amazon**)
3. Find the correct cover
4. Right-click the image → **Copy image address**
5. Click **✎ Edit** → paste the URL in the cover field → **Apply** → **Save**

Or upload an image from your device directly via the 📷 overlay that appears when hovering over the cover in edit mode.

---

## Data & privacy

- Your collection data is stored in your Supabase project — a database you control
- Cover images you upload go to your Supabase Storage bucket — also under your control
- The app makes requests to iTunes Search API (for search and covers) and MusicBrainz (for barcode lookup) — both are read-only, no data is sent
- No analytics, no tracking, no ads

---

## File structure

```
cd-collection.html    # The entire app — one self-contained file
README.md             # This file
```

That's it. The app is intentionally a single HTML file so it's easy to share, back up, and deploy anywhere.

---

## Tech stack

| Thing | What it does |
|---|---|
| Vanilla JS | Everything — no framework |
| iTunes Search API | Album search and cover art |
| MusicBrainz API | Barcode lookup |
| Supabase JS SDK | Cloud database and storage |
| ZXing Browser | Barcode scanning fallback for Firefox |
| Web BarcodeDetector API | Native barcode scanning (Chrome/Safari/Edge) |
| localStorage | Offline-first data persistence |
| Web Crypto API | Password hashing (SHA-256) |
| Canvas API | Image compression before upload |

---

## Customization

The app uses CSS variables throughout. To change the color scheme, edit these values in the `<style>` block:

```css
:root {
  --bg: #0e0d0c;           /* page background */
  --surface: #1a1916;      /* card backgrounds */
  --gold: #c9a84c;         /* accent color */
  --cream: #f0ead8;        /* primary text */
}
```

The display font is [Syne](https://fonts.google.com/specimen/Syne), body is [DM Sans](https://fonts.google.com/specimen/DM+Sans), and monospace elements use [DM Mono](https://fonts.google.com/specimen/DM+Mono) — all loaded from Google Fonts.

---

## License

MIT. Do whatever you want with it.
