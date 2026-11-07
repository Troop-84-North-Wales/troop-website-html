# Major Folders Explained

## `/css/` — Stylesheet Directory

### Contents:
- `style.css` — Single stylesheet for all pages

### Purpose:
- Contains all CSS rules for the website
- Colors and theme defined at top (under `:root`)
- Responsive design breakpoints
- Navigation, buttons, forms styles

### How to Edit:
Open `css/style.css` in any text editor. Change color values or add new rules as needed. All pages automatically inherit these styles.

---

## `/js/` — JavaScript Directory

### Contents:
- `config.js` — **ALL SETTINGS** (troop name, meeting info, links)
- `main.js` — Site behavior (no need to edit)

### Purpose:
- `config.js`: Populates dynamic content across all pages
- `main.js`: Handles JavaScript functionality (loaded by all HTML pages)

### How to Edit:
**Only edit `config.js`** for dynamic content. Changes take effect immediately after saving and refreshing the browser.

---

## `/logo/` — Logo Directory

### Contents:
- `84Logo.png` — Troop logo image

### Purpose:
- Displayed on header/nav across all pages
- Can be replaced with any image format (PNG, SVG, JPG)

---

## `/docs/` — Documentation Folder

### Contents:
- `architecture.md` — Repository structure overview
- `entry_points.md` — How to access the site
- `package_json.md` — Why there's no package.json
- `folders.md` — This file (folder documentation)
- `beginner_guide.md` — Beginner-friendly guide

### Purpose:
- Centralized documentation for developers and volunteers
- Can be published alongside the website or kept internal

---

## `/` — Root Directory Files

### HTML Pages:
- `index.html` — Home page
- `about.html` — About page
- `calendar.html` — Calendar page
- `photos.html` — Photos page

### Other Files:
- `README.md` — Main project documentation
- `.gitignore` — Git ignore rules (OS files, editor files, etc.)

---

## File Relationships

```
index.html
├── references css/style.css (in <link> tag)
└── references js/main.js (in <script> tag)

js/main.js
└── loads settings from js/config.js
```

All HTML pages share the same CSS and JavaScript, reducing duplication.

---

## Deployment Notes

When uploading to a web host:

1. Upload **all folders and files** (don't compress into ZIP)
2. Keep the folder structure intact
3. Root path should be `/` or `/troop-website-html/`
4. `index.html` is the home page entry point
