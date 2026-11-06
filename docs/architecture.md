# Architecture

This is a **static website with no build step**. There is no backend, no database, no framework, and no `package.json`. The site is pure HTML, CSS, and JavaScript — the browser renders it directly.

## High-level architecture diagram

```
index.html  about.html  calendar.html  photos.html  history.html
        │            │              │              │
        └────────────┴──────────────┴──────────────┘
                     │  shared HTML shell (header + nav + footer)
                     ▼
                css/style.css        (one shared stylesheet)
                     ▼
                js/config.js         ← ALL SETTINGS (the only file to edit)
                     ▼
                js/main.js           ← renderer / behavior
                     ▼
                logo/84Logo.png      ← brand image (favicon, header, footer)
```

## Key design principles

1. **No build step.** Files are served as-is. There is nothing to compile, minify, or bundle.
2. **Single stylesheet.** One `css/style.css` serves all five pages. Every visual change lives in that file (or in the `:root` design tokens).
3. **Single configuration file.** One `js/config.js` drives almost every visible value on the site. This is the "code-to-edit" ratio that keeps maintenance trivial.
4. **Two JavaScript files with distinct jobs:**
   - `config.js` — *data*. The configuration seam. You should edit this.
   - `main.js` — *behavior*. The renderer. Normally you don't touch this.
5. **Progressive enhancement via data attributes.** Pages mark elements with attributes like `data-field`, `data-href`, `data-email`, `data-year`; `main.js` fills them at runtime from `config.js`. This lets a single `config.js` update the header, hero, footer, and meeting info on every page at once.
6. **Responsive by CSS alone** — liquid sizing (`clamp()`, `min()`), with a mobile nav toggle (no JS framework needed) triggered by `main.js`.

## Component breakdown

### HTML (5 pages)
`index.html`, `about.html`, `history.html`, `calendar.html`, `photos.html`.

Each page has the same shell:
- `<header>` with brand (logo + text), `<nav>` (5 links), and a mobile `.nav-toggle` button.
- `<main>` with page-specific sections and a `#join` call-to-action on the home page.
- `<footer>` with identity, meetings, links, and a year placeholder `© <span data-year>`.
- Ends with `<script src="js/config.js">` then `<script src="js/main.js">`.

### CSS (`css/style.css`)
- **`:root` design tokens** — navy, gold, tan, ink, muted, and radius/shadow values used as CSS variables.
- Header/nav (sticky, bordered, gold accent), brand (logo + text), mobile nav toggle.
- Hero (gradient), section cards, buttons (gold/navy/outline), grid, footer, and responsive rules.
- No build output — hand-written, plain CSS.

### JavaScript (`js/config.js` + `js/main.js`)
- **`config.js`** defines `window.TROOP_CONFIG` — a plain object holding all settings: troop basics, meetings, contact email, calendar ID, photo gallery URLs, and external links.
- **`main.js`** (an IIFE, `"use strict"`) reads `window.TROOP_CONFIG`, then:
  - Fills `[data-field="key"]` textContent from `C[key]`.
  - Sets `[data-href="key"]` to `C[key]`.
  - Wires `[data-email]` to `mailto:<contactEmail>` and shows/hides it.
  - Writes `new Date().getFullYear()` into `[data-year]`.
  - Toggles `.site-nav.open` for the mobile nav button.
  - **Calendar page:** builds an `<iframe>` embed from `googleCalendarId` (and `calendarTimeZone`), or shows the empty "not connected" state.
  - **Photos page:** fills the Facebook/Google photo card links and hides the empty state when a gallery is configured.

### Assets (`logo/`)
- `84Logo.png` — the troop logo. Used as the favicon, the header brand, and the footer logo across all pages.

---

## Entry points

There is **no package.json and no Node entry point**. The "entrypoints" are the files the browser loads.

### Pages (UI entry points)
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
| `css/style.css` | Shared stylesheet — colors, layout, buttons, cards, responsive nav |
| `js/config.js` | The single source of truth for ALL settings |
| `js/main.js` | Renderer + behavior — fills `data-field` / `data-href` / `data-email` / `data-year`, mobile nav, calendar embed, photo gallery cards |
| `logo/84Logo.png` | Troop logo — favicon, header brand, footer logo |

### The browser's literal load sequence
1. `index.html` — HTML shell, `<link rel="stylesheet" href="css/style.css">` and `<link rel="icon" href="logo/84Logo.png">`.
2. `css/style.css` — styling applied.
3. `logo/84Logo.png` — brand image.
4. `js/config.js` — defines `window.TROOP_CONFIG`.
5. `js/main.js` — reads `window.TROOP_CONFIG`, fills the page, wires interactivity.

### Runtime entry point (for a developer)
`js/config.js` is where any change begins. It is the configuration seam of the entire site.
