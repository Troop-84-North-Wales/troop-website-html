# Reference — Folder-by-folder

## `css/`
| File | Size | Purpose |
|---|---|---|
| `style.css` | 7.8 KB | The **only** stylesheet. Contains a `:root` design-token block (navy `#1b2a4a`, gold `#d4a534`, tan `#f4ecdc`, ink `#1f2430`) used as CSS variables. Defines the header/nav, hero, buttons, cards, grid, footer, and responsive rules. |

## `js/`
| File | Size | Purpose |
|---|---|---|
| `config.js` | 2.2 KB | **The single settings file.** Defines `window.TROOP_CONFIG` — all settings (troop basics, meetings, contact email, calendar ID, photo gallery URLs, external links). The only file you normally edit. |
| `main.js` | 3.2 KB | **The renderer / behavior.** Reads `window.TROOP_CONFIG`, fills `[data-field]`/`[data-href]`/`[data-email]`/`[data-year]` attributes, wires the mobile nav toggle, builds the calendar `<iframe>`, and wires the photo gallery cards. Do not edit normally. |

## `logo/`
| File | Size | Purpose |
|---|---|---|
| `84Logo.png` | 209 KB | Troop 84 logo. Used as the favicon, header brand, and footer logo across all pages. Referenced at `width="339" height="353"`. |

## Root files

| File | Purpose |
|---|---|
| `index.html` | **Home page.** Hero, weekly meetings (boys 84B + girls 84G), join CTA. |
| `about.html` | About the troop + how to visit. |
| `history.html` | Troop history. |
| `calendar.html` | Embedded Google Calendar, with a fallback "not connected" empty state. |
| `photos.html` | Facebook + Google Photos gallery cards, with a fallback empty state. |
| `README.md` | This site's own README (the site's description, not the repo docs). |
| `.gitignore` | Ignores OS/editor artifacts (`.DS_Store`, `.vscode/`, `*.swp`, etc.). |

## `docs/` (new — this documentation)

| File | Purpose |
|---|---|
| `README.md` | Beginner-friendly guide + quick index. |
| `architecture.md` | Architecture, principles, data flow, and entry points. |
| `package-json.md` | Explanation of why there is no `package.json` and how to run/deploy. |
| `reference.md` | This file — folder-by-folder reference. |

---

## Settings reference (from `js/config.js`)

| Setting | Type | Default | Where it appears |
|---|---|---|---|
| `troopName` | string | `"Troop 84"` | Header brand text, hero `<h1>` |
| `troopFullName` | string | `"BSA Troop 84 North Wales, PA"` | Footer heading |
| `town` | string | `"North Wales, PA"` | Header brand subtitle |
| `meetingDay` | string | `"Thursday evenings"` | Hero, calendar page, footer |
| `meetingTime` | string | `"7:30–9:00 pm"` | Hero, calendar page, footer |
| `meetingPlace` | string | `"North Wales Scout Center"` | Hero, footer |
| `meetingAddress` | string | `"300 Church Street, North Wales, PA"` | Hero, footer |
| `contactEmail` | string | `"Troop84NWales@Gmail.com"` | Email "button" (`mailto:`); empty string hides it |
| `googleCalendarId` | string | `"5tespi39ansmoqgd7onj1u3dh8@group.calendar.google.com"` | Calendar embed source |
| `calendarTimeZone` | string | `"America/New_York"` | Calendar iframe time-zone |
| `facebookPhotosUrl` | string | `https://www.facebook.com/Troop84NWalesPA/photos` | Facebook gallery link |
| `googlePhotosUrl` | string | `"PASTE_GOOGLE_PHOTOS_ALBUM_URL_HERE"` | Google Photos link; **`PASTE_`** prefix = hidden |
| `scoutsBsaInfoUrl` | string | `https://www.scouting.org/programs/scouts-bsa/` | External "About Scouts BSA" link |
| `originalSiteUrl` | string | `https://www.troop84northwalespa.org/` | "Current Troop 84 website" link in footer |

---

## Quick map of the customization surface

| Want to change… | Edit this file |
|---|---|
| Troop name / town / full name | `js/config.js` → `troopName`, `troopFullName`, `town` |
| Meeting day/time/place/address | `js/config.js` → `meeting*`, `contactEmail` |
| Calendar | `js/config.js` → `googleCalendarId` |
| Photo galleries | `js/config.js` → `facebookPhotosUrl`, `googlePhotosUrl` |
| External links | `js/config.js` → `scoutsBsaInfoUrl`, `originalSiteUrl` |
| Page text / content | `.html` files |
| Colors / layout | `css/style.css` → `:root` variables and component rules |
| Logic / behavior (rarely) | `js/main.js` |

---

## Related (site-level)

- The repo's root `README.md` is the **site's own README** (who it's for, how to open it, how to deploy). The `docs/` folder here is **this repository's documentation**.
