# Entry Points

## Browser Entry Points

### 1. `index.html` — Home Page (Primary Entry)
- **Purpose**: Welcome message, troop info, meeting details, "join call" CTA
- **URL path**: `/` or `/index.html`
- **What it does**: 
  - Displays header with troop name/logo
  - Shows meeting information
  - Provides navigation to other pages
  - Includes call-to-action buttons (email contact, join meeting)

### 2. `about.html` — About Page
- **Purpose**: Troop history, mission, how to visit
- **URL path**: `/about.html`

### 3. `calendar.html` — Calendar Page
- **Purpose**: Embedded Google Calendar showing troop meetings/events
- **URL path**: `/calendar.html`
- **Note**: Requires valid `googleCalendarId` in `js/config.js`

### 4. `photos.html` — Photos Page
- **Purpose**: Links to Facebook and/or Google Photos albums
- **URL path**: `/photos.html`
- **Note**: Requires valid photo album URLs in `js/config.js`

## JavaScript Entry Points

### `js/main.js`
- **Purpose**: Site behavior (no direct entry needed)
- **Loaded by**: All HTML pages via `<script src="js/main.js">`
- **What it does**: 
  - Reads settings from `config.js`
  - Populates dynamic content (troop name, meeting info, links)
  - Handles any interactive features

### `js/config.js` — Configuration Entry
- **Purpose**: Centralized settings for all pages
- **Where to edit**: This file contains ALL dynamic content
- **Key variables**:
  ```javascript
  {
    troopName: "...",
    troopFullName: "...",
    town: "...",
    meetingDay: "...",
    meetingTime: "...",
    meetingPlace: "...",
    meetingAddress: "...",
    contactEmail: "...",
    googleCalendarId: "...",
    calendarTimeZone: "America/New_York",
    facebookPhotosUrl: "...",
    googlePhotosUrl: "..."
  }
  ```

## Development Entry Points

### Local Preview
```bash
python3 -m http.server 8000
# Visit http://localhost:8000
```

### Build (not required)
This site has no build step. If you add a bundler later:
```bash
# Example with Vite (optional, not currently used)
npm install -D vite
npx vite
```

## Deployment Entry Points

### GitHub Pages
1. Push to `main` branch
2. Enable GitHub Pages in repo settings
3. Site available at `https://yourusername.github.io/troop-website-html/`

### Netlify/Cloudflare Pages
1. Drag-and-drop the folder to Netlify drop zone
2. Or connect Git repository
3. Site goes live immediately
