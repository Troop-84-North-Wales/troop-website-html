# Troop 84 Website — Documentation

> **Repository:** `troop84-website-html`
> **Type:** Static HTML/CSS/JS website (no build step, no Node dependencies)
> **Subject:** BSA Troop 84, North Wales, PA
> **Run it:** open `index.html` in a browser (double-click works) OR run `python3 -m http.server 8000` and visit `http://localhost:8000`

---

## Quick start (beginner-friendly)

1. **Open the site.** Double-click `index.html`. It works straight from your file system (`file://`).
2. **Make changes.** The site has exactly **two files you need to edit**:
   - `js/config.js` — every setting (troop name, meeting times, calendar, photo links). This is the *only* file you normally touch.
   - `*.html` pages — page text, titles, and content (only if you add/change written content).
3. **Refresh the browser** to see changes.
4. **Deploy.** Copy the whole folder to any web host (cPanel/FTP, GitHub Pages, Netlify drag-and-drop, Cloudflare Pages). `index.html` is the home page.

**Run a local server (so assets load with `/` paths):**

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

---

## Files at a glance

| File / Folder | Purpose |
|---|---|
| `index.html` | Home page |
| `about.html` | About the troop + how to visit |
| `calendar.html` | Embedded Google Calendar |
| `photos.html` | Facebook & Google Photos galleries |
| `history.html` | Troop history |
| `css/style.css` | Shared styles (colors, layout, responsive) |
| `js/config.js` | **ALL SETTINGS** (edit this) |
| `js/main.js` | Page behavior (normally don't edit) |
| `logo/84Logo.png` | Troop 84 logo (also favicon) |
| `README.md` | This site's own README (start here) |
| `.gitignore` | Ignores OS/editor artifacts |

> For a full folder-by-folder reference, see `reference.md`.

---

## How it works (one-paragraph summary)

This is a **serverless static site**. There is no backend, no database, and no build step. Every page is plain HTML, styled by one shared `css/style.css`, and given its "settings" (troop name, meeting info, calendar, photo links) by a tiny JavaScript file, `js/config.js`. `js/main.js` then reads those settings and fills in the HTML automatically.

The one clever (and important) part is **how settings reach the HTML**: pages mark elements with attributes like `data-field="troopName"` and `data-href="scoutsBsaInfoUrl"`, and `main.js` copies the matching values from `config.js` into those elements at runtime. That means changing a single value in `config.js` updates the header, hero, footer, and meeting info on every page at once.

---

## Table of contents

- [1. Architecture](#1-architecture)
- [2. Entry points](#3-entry-points)
- [3. package.json](#5-packagejson)
- [4. Folder-by-folder reference](#4-folder-by-folder-reference)
- [5. Beginner-friendly guide](#5-beginner-friendly-guide)

---

## Beginner-friendly guide

### What this is
A small, no-frills website for a Scouts BSA troop. It has 5 pages (Home, About, History, Calendar, Photos), one shared stylesheet, and two JavaScript files. There is no framework, no compiler, and no database — just pure HTML/CSS/JS that runs in your browser.

### How to run it locally
1. Open a terminal in this folder.
2. Run `python3 -m http.server 8000` (or `python` if `python3` isn't on PATH).
3. Visit `http://localhost:8000`.

### How to edit the site

**Settings (most changes):**
Open `js/config.js` in any text editor. Change the values inside the quotes. Save. Refresh the browser.

```js
troopName: "Troop 84",
meetingDay: "Thursday evenings",
meetingTime: "7:30–9:00 pm",
googleCalendarId: "5tespi39ansmoqgd7onj1u3dh8@group.calendar.google.com",
facebookPhotosUrl: "https://www.facebook.com/Troop84NWalesPA/photos",
```

**Page text:**
Open the relevant `.html` file (e.g., `index.html`) and edit the readable text between the tags.

**Photos & Calendar:**
- **Calendar:** Go to Google Calendar → **⋮ → Settings and sharing → Make available to public** → copy the **Calendar ID** (e.g. `…@group.calendar.google.com`) → paste into `js/config.js` as `googleCalendarId`.
- **Facebook photos:** Copy the album URL from `facebookPhotosUrl` (set the album to Public).
- **Google Photos:** Click **Share → Create link**, paste into `js/config.js` as `googlePhotosUrl`.

> Note: if a `googlePhotosUrl` starts with `PASTE_`, the Google Photos card stays hidden. Fill it in to enable it.

### How to deploy
Upload the **entire folder** to any web host:

- cPanel / FTP / file manager
- **GitHub Pages**, **Netlify** (drag-and-drop the folder), **Cloudflare Pages**, **Vercel**

`index.html` is the home page.

### Common questions

- **"Why do I see placeholder text?"** Because a setting is still a placeholder (e.g., a `PASTE_...` value) or empty. Fill it in `js/config.js`.
- **"How do I add a page?"** Copy an existing `.html` file, add a link to it in `<nav>` on every page, and you're done (no build step).
- **"Where do the photos go?"** The site links to Facebook/Google Photos galleries — photos are *not* stored in this repo.
- **"Is there a database?"** No. Everything is static files.
- **"What should I NOT edit?"** `js/main.js` — behavior is driven by `config.js`; `main.js` is the renderer. Leave it alone.

---

## Architecture

The site follows a **static-site layering** pattern:

```
index.html  about.html  calendar.html  photos.html  history.html
        │            │              │              │
        └────────────┴──────────────┴──────────────┘
                     │  shared shell (header/footer/nav)
                     ▼
                css/style.css        (one shared stylesheet)
                     ▼
                js/config.js   ← ALL SETTINGS (the only thing to edit)
                     ▼
                js/main.js     ← renderer (fills data-field / data-href)
                     ▼
                logo/84Logo.png  ← brand image (favicon, header, footer)
```

- **No build step.** Files are served as-is.
- **One stylesheet** serves all five pages; every visual change lives in `css/style.css` (or `:root` variables).
- **One configuration file** serves all settings; this is the "code-to-edit" ratio that keeps maintenance trivial.
- **Behavior is minimal** — pure vanilla JS (no framework, no dependencies, no Node).

### Data flow: how a page gets rendered

1. Browser loads `index.html`.
2. `<script src="js/config.js">` runs first, defining `window.TROOP_CONFIG`.
3. `<script src="js/main.js">` runs next. It reads `window.TROOP_CONFIG` into a local variable `C`.
4. For each element with a `data-field` attribute, `main.js` set textContent from `C[key]`.
5. For each element with `data-href`, it sets `href` from `C[key]`.
6. For `[data-email]`, it sets `href="mailto:…"` and shows/hides the element.
7. For `[data-year]`, it sets the year text.
8. For the calendar page, it builds an `<iframe>` from `googleCalendarId` and `calendarTimeZone`.
9. For the photos page, it fills `card-facebook` / `card-google` links and hides the empty state when a gallery is configured.

### Responsive behavior

- Desktop: horizontal nav + three-column hero grid.
- Mobile: `.nav-toggle` button reveals the nav (toggled by `main.js` via a CSS class `.site-nav.open`).
- Font sizes and layout use `clamp()` / `min()` for fluid scaling.
- The logo is `84Logo.png` and is referenced by `width="339" height="353"` in every page.

---

## Entry points

There is **no package.json and no Node entry point**. The "entrypoints" are the files the browser loads:

### Pages (the UI entry points)
| File | Role |
|---|---|
| `index.html` | **Home page** — hero, meetings, join CTA, "new to scouting" band |
| `about.html` | About both troops (boys 84B, girls 84G) + how to visit |
| `history.html` | Troop history |
| `calendar.html` | Embedded Google Calendar (with fallback "not connected" state) |
| `photos.html` | Facebook + Google Photos gallery links (with empty state) |

### Asset entry points (loaded by every page)
| File | Role |
|---|---|
| `css/style.css` | **One shared stylesheet** — colors, layout, buttons, cards, responsive nav |
| `js/config.js` | **The single source of truth for ALL settings** (troop info, meeting info, calendar ID, photo URLs, external links) |
| `js/main.js` | **Renderer + behavior** — fills data-field/data-href attributes, email buttons, footer year, mobile nav toggle, calendar embed, photo gallery cards |
| `logo/84Logo.png` | Troop 84 logo — used as favicon, header brand, and footer logo |

### How a page is loaded (the browser's literal entry sequence)

1. `index.html` — HTML shell, links `<link rel="stylesheet" href="css/style.css">` and `<link rel="icon" href="logo/84Logo.png">`.
2. `css/style.css` — styling applied.
3. `logo/84Logo.png` — brand image.
4. `js/config.js` — defines `window.TROOP_CONFIG` with all settings.
5. `js/main.js` — reads `window.TROOP_CONFIG`, fills in the page, and wires up interactivity.

### Runtime entry point (for a developer)
`js/config.js` is where any change begins. It's the "configuration seam" of the entire application.

---

## package.json

**There is no `package.json`, `package-lock.json`, or `node_modules`.**

This project does **not** use Node.js or a build system. It is a pure static site, so there is nothing to install and nothing to build.

### Why that matters
- No `npm install` needed.
- No `npm run dev` / `npm run build` commands.
- The site just works by opening the HTML file.

### `.gitignore` foreshadows a possible future
The existing `.gitignore` lists `node_modules/`, `.next/`, `dist/`, `build/`, `.vercel/`, and `*.log`. This strongly hints the site *could* have been migrated to a Node-based build (Next.js, Vite, etc.) at some point, but **currently it is deliberately kept plain**. If you later want to add a framework, you'd add `package.json` and update the build pipeline — but that's out of scope for this site as it exists today.

### What you *would* do if it DID have a package.json
(If asked later.) A typical setup would be:
- `npm install` to pull dependencies.
- `npm run dev` to start a local dev server.
- `npm run build` to produce static output for deployment.

Since none of that exists here, just skip to the manual workflow (edit HTML/CSS/JS, deploy the folder).

---

## Folder-by-folder reference

### `css/`
- `style.css` (7.8 KB) — **the only stylesheet**. Contains a `:root` design-token block (navy `#1b2a4a`, gold `#d4a534`, tan `#f4ecdc`, ink `#1f2430`, etc.) used as CSS variables across the whole site. Also defines the header/nav, hero, buttons, cards, grid layout, footer grid, and responsive rules.

### `js/`
- `config.js` (2.2 KB) — **the single settings file**. Defines `window.TROOP_CONFIG` object with:
  - Troop basics: `troopName`, `troopFullName`, `town`
  - Meetings: `meetingDay`, `meetingTime`, `meetingPlace`, `meetingAddress`
  - Contact: `contactEmail` (empty string hides the email button)
  - Calendar: `googleCalendarId`, `calendarTimeZone`
  - Photo galleries: `facebookPhotosUrl`, `googlePhotosUrl`
  - Links: `scoutsBsaInfoUrl`, `originalSiteUrl`
- `main.js` (3.2 KB) — **the renderer/behavior**. FYI, the key functions: `isSet(v)` (ignores `PASTE_` placeholders), `[data-field]` text filling, `[data-href]` href filling, `[data-email]` mailto wiring, `[data-year]` year injection, mobile nav toggle, calendar `<iframe>` embed, photo gallery card wiring.

### `logo/`
- `84Logo.png` (209 KB) — the Troop 84 logo. Used as:
  - Favicon (`<link rel="icon" href="logo/84Logo.png">`)
  - Header brand image
  - Footer logo
  - Referenced at `width="339" height="353"` in each page.

### Root files
- `index.html`, `about.html`, `history.html`, `calendar.html`, `photos.html` — the five pages.
- `README.md` — this site's own README (the site's description, not the repo docs).
- `.gitignore` — ignores OS/editor artifacts (`.DS_Store`, `.vscode/`, `*.swp`, etc.).
- `.git/` — git history (the site's version control).

---

## Beginner-friendly guide (quick reference)

### Open the site
- **Fastest:** double-click `index.html` (works from `file://`).
- **With a server (recommended for assets like `/css/style.css`):** `python3 -m http.server 8000` then open `http://localhost:8000`.

### Make a change
1. Edit `js/config.js` for any setting.  
2. Edit `.html` pages for written content.  
3. Refresh the browser.

### Add a page
1. Copy an existing `.html` file (e.g., `about.html`).
2. Open it in a text editor and rename it (e.g., `newpage.html`).
3. Update the `<title>` and heading text.
4. Add a `<li><a href="newpage.html">New Page</a></li>` to the `<nav>` list in **every** page.
5. Save all files and test in the browser.

### Add a photo gallery
1. In `js/config.js`, set `facebookPhotosUrl` or `googlePhotosUrl`.
2. Leave `googlePhotosUrl` as `""` (or `PASTE_...`) to hide the Google Photos card.
3. Save and refresh `photos.html`.

### Deploy
1. Zip the folder (or upload via FTP/file manager).
2. Drop it on any host: GitHub Pages, Netlify drag-and-drop, Cloudflare Pages, or your existing web host.
3. Set the site's root to this folder. `index.html` is the home page.

---

## Notes for testing / validation

- **No automated tests.** This is a static site; validation is manual (open each page in a browser, check that settings filled in correctly, check the calendar/photos cards appear or hide as expected).
- **Key validations:** header/footer text matches `config.js`; email button appears/hides with `contactEmail`; calendar card appears with `googleCalendarId`; photo cards appear/hide with `facebookPhotosUrl`/`googlePhotosUrl`; mobile nav toggle works.
- **Fonts/icons:** the site uses system fonts (no webfonts) and emoji icons (🏕️ 🧭 📅 📘 🖼️).

---

*Documented against the repository at `C:\Users\abhijit\troop84\troop-website-html`. No files in the website itself were modified.*
