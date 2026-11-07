# package.json (Not Present)

## Why There's No `package.json`

This repository **does not have a `package.json` file**. This is intentional and by design:

- It's a **static HTML/CSS/JS website** — no Node.js dependencies
- No build tools needed (Webpack, Vite, Parcel, etc.)
- No transpilation (Babel) or bundling required
- Files are served directly as plain HTML

## What You See Instead

### `README.md` — Project Documentation
The project uses `README.md` instead of `package.json` for:
- Project description
- Setup instructions
- Configuration guide
- Deployment steps

### `css/style.css` — Stylesheet
Contains all CSS styling:
- Colors defined at top (under `:root`)
- Responsive design rules
- Navigation styles
- Button and form styles

### `js/config.js` — Dynamic Content
All dynamic content lives here:
- Troop name and logo
- Meeting information
- Contact email
- Calendar embed URL
- Photo album links

### `js/main.js` — Site Behavior
Handles site-wide JavaScript functionality:
- Populating dynamic content from `config.js`
- Any interactive features (if added later)

## Adding Dependencies (Optional)

If you later want to add features that require Node.js:

1. Initialize the project:
```bash
npm init -y
```

2. Install dependencies as needed:
```bash
npm install <package-name>
```

3. Add scripts to `package.json`:
```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  }
}
```

4. Run development server:
```bash
npm run dev
```

**Note**: These are optional and not currently needed for this static site.

## Current State

| File | Purpose | Present? |
|------|---------|----------|
| `package.json` | Node.js dependencies | ❌ No (by design) |
| `README.md` | Project docs | ✅ Yes |
| `css/style.css` | Stylesheet | ✅ Yes |
| `js/config.js` | Dynamic content | ✅ Yes |
| `js/main.js` | Site behavior | ✅ Yes |
