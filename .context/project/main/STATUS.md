# STATUS - main

## Current State

**Status:** Production
**Version:** v1.2.0
**Health:** Stable
**Last Deploy:** 2025-10-24

## What's Working

### Core Functionality
- [x] Text input (paste and manual)
- [x] Auto-process on paste
- [x] Manual "Touch Up" button
- [x] "Try Again" for variations
- [x] Auto-copy to clipboard
- [x] Clipboard icon button

### Settings
- [x] API token storage
- [x] Auto-process toggle
- [x] Force language option
- [x] Language selection
- [x] Hard reset (preserves token)

### PWA Features
- [x] Offline support (Service Worker)
- [x] iOS installation
- [x] Android installation
- [x] App icons (SVG)
- [x] Standalone display mode

### UI/UX
- [x] Dark theme
- [x] iOS safe area support
- [x] Toast notifications (top)
- [x] Settings modal
- [x] Mobile-optimized layout

## Known Issues

*None currently tracked*

## Recent Changes

| Version | Date | Summary |
|---------|------|---------|
| v1.2.0 | 2025-10-24 | Fixed manifest paths, renamed to Clipot |
| v1.1.2 | 2025-10-24 | Hard reset keeps API token |
| v1.1.1 | 2025-10-24 | Toast moved to top |
| v1.1.0 | 2025-10-24 | iOS fixes, clipboard icon |

## Dependencies Status

| Dependency | Status | Notes |
|------------|--------|-------|
| Anthropic API | OK | claude-haiku-4-5-20251001 |
| GitHub Pages | OK | Primary deployment |

## Deployment

- **Primary:** GitHub Pages
- **Alternatives:** Netlify, Vercel, Cloudflare Pages
- **Deploy method:** `git push origin main`

## Metrics

- **Bundle size:** ~32KB (index.html)
- **Files count:** 5 core files
- **Dependencies:** 0 npm packages

## Next Steps

1. Monitor for user feedback
2. Consider adding text history feature
3. Explore desktop PWA optimizations
