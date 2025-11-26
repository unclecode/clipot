# Design Principles

## Core Philosophy

**Keep it simple.** This app succeeds because of what it doesn't do.

## Principles

### 1. Zero Dependencies

- No npm packages
- No build process
- No frameworks
- Pure vanilla JavaScript
- Deploy by copying files

**Why:** Maximum simplicity, zero maintenance burden, works forever.

### 2. Single File Architecture

- All code in `index.html`
- Inline CSS and JavaScript
- No module bundling
- No asset pipeline

**Why:** Easy to understand, modify, and deploy. No build tools to break.

### 3. Mobile-First Design

- 430px max-width (phone optimized)
- Touch-friendly buttons
- iOS safe area support
- Portrait orientation default

**Why:** Primary use case is quick text polishing on mobile devices.

### 4. Offline-First

- Service worker caches app
- Works without internet (after first load)
- API calls are the only online requirement

**Why:** PWA should work like a native app.

### 5. Privacy by Design

- All data stored locally (localStorage)
- No intermediary server
- Direct API calls to Anthropic
- No analytics or tracking

**Why:** Users' text is personal. No data should leak.

### 6. Minimal UI

- One main action: "Touch Up"
- One alternative: "Try Again"
- Settings hidden behind gear icon
- No clutter, no distractions

**Why:** The app has one job. The UI should reflect that.

## Anti-Patterns to Avoid

- Adding a backend server
- Adding npm dependencies
- Splitting into multiple files (unless truly necessary)
- Adding features that don't serve the core use case
- Over-engineering for hypothetical future needs
