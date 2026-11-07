# Repository Architecture

## Overview

This is a **static HTML/CSS/JS website** for BSA Troop 84 (North Wales, PA). It requires no build tools, Node.js, or server-side code. Open any `.html` file directly in a browser or serve it with a simple HTTP server.

## Directory Structure

```
troop-website-html/
├── index.html          # Home page (welcome, meeting info, CTA)
├── about.html          # About the troop + visiting info
├── calendar.html       # Embedded Google Calendar
├── photos.html         # Links to photo albums (Facebook/Google Photos)
├── css/
│   └── style.css       # Shared styles (colors at :root)
├── js/
│   ├── config.js       # ← ALL SETTINGS: troop name, meeting info, links
│   └── main.js         # Site behavior (no need to edit)
├── logo/
│   └── 84Logo.png      # Troop logo
├── docs/               # Documentation folder (this location)
├── README.md           # Project overview and setup guide
└── .gitignore          # Git ignore rules
```

## Key Characteristics

- **No build step** — files are served as-is
- **Plain HTML5** — no React, Vue, or other frameworks
- **Single-page structure** — each page is a separate `.html` file
- **Centralized configuration** — all dynamic content lives in `js/config.js`
- **Static assets** — CSS, JS, and images are referenced directly

## How It Works

1. Browser loads an `.html` file (e.g., `index.html`)
2. HTML references shared `css/style.css` and `js/main.js`
3. JavaScript (`main.js`) uses settings from `config.js` to populate dynamic content
4. No compilation, bundling, or transpilation occurs

## Deployment

Upload the entire folder to any web host:
- FTP/SFTP with file manager
- GitHub Pages (push to branch, enable Pages)
- Netlify/Cloudflare Pages (drag-and-drop)
- Local file:// (double-click `index.html`)
