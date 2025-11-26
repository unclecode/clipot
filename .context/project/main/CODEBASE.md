# CODEBASE - main

## Directory Structure

```
touch-up-grammar/
├── .context/                 # Project planning (AI-readable)
│   └── project/
│       ├── INDEX.md
│       ├── WORKFLOW.md
│       ├── PRINCIPLES.md
│       ├── LESSONS.md
│       ├── main/
│       │   ├── INDEX.md
│       │   ├── TODO.md
│       │   ├── STATUS.md
│       │   ├── CODEBASE.md
│       │   ├── CHANGELOG.md
│       │   └── docs/
│       │       ├── README.md
│       │       └── DEPLOY.md
│       └── archive/
│
├── index.html               # Main application (955 lines, 32KB)
├── manifest.json            # PWA manifest configuration
├── service-worker.js        # Offline support
├── icon-192.svg            # App icon (192x192)
├── icon-512.svg            # App icon (512x512)
├── install.html            # Installation guide page
├── .gitignore              # Git ignore rules
│
└── [Development files]      # Ignored in .gitignore
    ├── generate-icons.js
    ├── create-png-icons.html
    ├── gen-icon.py
    ├── icon-192.png.base64
    └── INSTALL_SUMMARY.txt
```

## Core Files

### index.html (Main Application)

**Location:** `/index.html`
**Size:** ~32KB, 955 lines
**Contains:** Complete application (HTML + CSS + JavaScript)

**Structure:**
```html
<!DOCTYPE html>
<html>
<head>
  <!-- Meta tags, viewport, PWA config -->
  <style>
    /* All CSS styles (~200 lines) */
  </style>
</head>
<body>
  <!-- UI components -->
  <div id="app">...</div>
  <div id="settings-modal">...</div>

  <script>
    /* All JavaScript (~400 lines) */
    // - Settings management
    // - API integration
    // - UI handlers
    // - Clipboard operations
  </script>
</body>
</html>
```

**Key Sections:**
- Lines 1-50: Meta, viewport, PWA links
- Lines 50-250: CSS styles
- Lines 250-500: HTML structure
- Lines 500-955: JavaScript logic

### manifest.json

**Location:** `/manifest.json`
**Purpose:** PWA configuration

```json
{
  "name": "Clipot",
  "short_name": "Clipot",
  "start_url": "./index.html",
  "display": "standalone",
  "icons": [...]
}
```

### service-worker.js

**Location:** `/service-worker.js`
**Purpose:** Offline caching

**Key Logic:**
- Caches `index.html` on install
- Serves from cache first
- Skips API requests (no caching)

## Code Patterns

### API Integration

```javascript
// API call pattern (in index.html)
const response = await fetch('https://api.anthropic.com/v1/messages', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'x-api-key': apiToken,
    'anthropic-version': '2023-06-01',
    'anthropic-dangerous-direct-browser-access': 'true'
  },
  body: JSON.stringify({
    model: 'claude-haiku-4-5-20251001',
    max_tokens: 2000,
    messages: [{ role: 'user', content: prompt }]
  })
});
```

### Settings Storage

```javascript
// localStorage pattern
const settings = {
  get: (key) => localStorage.getItem(key),
  set: (key, value) => localStorage.setItem(key, value),
  remove: (key) => localStorage.removeItem(key)
};
```

### Toast Notifications

```javascript
// Toast pattern
function showToast(message, duration = 2000) {
  // Creates temporary notification at top of screen
}
```

## File Dependencies

```
index.html
├── manifest.json (linked via <link rel="manifest">)
├── service-worker.js (registered via JS)
├── icon-192.svg (referenced in manifest)
└── icon-512.svg (referenced in manifest)
```

## Build & Deploy

**No build process required.**

Deploy by copying these files:
1. `index.html`
2. `manifest.json`
3. `service-worker.js`
4. `icon-192.svg`
5. `icon-512.svg`

Or simply: `git push origin main` (GitHub Pages auto-deploys)
