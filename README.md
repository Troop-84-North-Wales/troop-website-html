# Troop 84 North Wales, PA — Static Website

A simple, no-build HTML/CSS/JS site for BSA Troop 84. It's a refreshed version of
https://www.troop84northwalespa.org/ with a Google Calendar page and a photo-gallery page.

## Files

```
troop84-site/
├── index.html      Home: welcome, meeting info, join call-to-action
├── about.html      About the troop + how to visit
├── calendar.html   Embedded Google Calendar
├── photos.html     Links to Facebook / Google Photos albums
├── css/style.css   Shared styles (colors at the top under :root)
├── js/config.js    ← ALL SETTINGS LIVE HERE
├── js/main.js      Site behavior (no need to edit)
└── README.md
```

## Open it

- **Easiest:** double-click `index.html` to open it in your browser (works from `file://`).
- **Local server (optional):** in this folder run `python3 -m http.server 8000` and visit http://localhost:8000

## Update settings — `js/config.js`

Open `js/config.js` in any text editor (Notepad, TextEdit, VS Code). Change the values
inside the quotes, save, and refresh the browser.

| Setting | What it does |
|---|---|
| `troopName`, `troopFullName`, `town` | Name shown in header, headings, footer |
| `meetingDay`, `meetingTime`, `meetingPlace`, `meetingAddress` | Meeting info shown on every page |
| `contactEmail` | Shows "Email the troop" buttons when set; hidden when `""` |
| `googleCalendarId` | **PLACEHOLDER** — your public Google Calendar ID (or full embed URL) |
| `calendarTimeZone` | Defaults to `America/New_York` |
| `facebookPhotosUrl` | **PLACEHOLDER** — Facebook album link |
| `googlePhotosUrl` | **PLACEHOLDER** — Google Photos shared album link |

Until a placeholder is replaced, the Calendar and Photos pages show a friendly
"not connected yet" message with instructions.

### Google Calendar

1. Open Google Calendar on a computer.
2. Next to the troop calendar, click **⋮ → Settings and sharing**.
3. Under **Access permissions for events**, check **Make available to public**
   (otherwise visitors will see an empty or "permission" calendar).
4. Scroll to **Integrate calendar** and copy the **Calendar ID**
   (e.g. `abc123@group.calendar.google.com`).
5. In `js/config.js` set:
   ```js
   googleCalendarId: "abc123@group.calendar.google.com",
   ```
   You can instead paste the full URL from **Embed code** (the `src="..."` part,
   starting with `https://calendar.google.com/calendar/embed?`).

### Photo galleries

- **Facebook:** open the album (set to Public), copy the address-bar URL into `facebookPhotosUrl`.
- **Google Photos:** open the album → **Share → Create link**, paste into `googlePhotosUrl`.
- Leave either one as the placeholder (or `""`) to hide that card.

## Edit page text

Each page is plain HTML. Open e.g. `index.html` and edit the text between tags.
Text with `data-field="..."` is filled from `config.js`, so change those in config instead.
The header/nav and footer are repeated in each page; if you add a page, copy an existing
page and add a link to the `<nav>` list in every file.

## Publish / deploy

The whole folder is the website — no build step. Upload the folder's contents to any web host:

- Your current host's file manager / FTP (upload everything, keep the folder structure)
- GitHub Pages, Netlify (drag-and-drop the folder), Cloudflare Pages, or Google Sites embed

`index.html` is the home page.
