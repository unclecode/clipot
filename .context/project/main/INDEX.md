# main - Core PWA Application

> The complete Clipot Progressive Web App

## Overview

This is the main (and only) subproject containing the entire Clipot PWA. It's a single-page application that provides AI-powered text refinement using Anthropic's Claude API.

## Quick Status

| Attribute | Value |
|-----------|-------|
| **Status** | Production |
| **Version** | v1.2.0 |
| **Last Updated** | 2025-10-24 |
| **Stability** | Stable |

## Features

- Text input via paste or manual entry
- Auto-process on paste (configurable)
- "Touch Up" button for manual processing
- "Try Again" for alternative refinements
- Auto-copy results to clipboard
- Settings panel for configuration
- Offline support via Service Worker
- PWA installation on iOS/Android

## Tech Stack

- **UI:** Vanilla JavaScript, inline CSS
- **API:** Anthropic Claude (claude-haiku-4-5-20251001)
- **Storage:** Browser localStorage
- **Offline:** Service Worker with cache-first strategy

## Key Files

| File | Purpose |
|------|---------|
| `index.html` | Complete application (HTML + CSS + JS) |
| `manifest.json` | PWA configuration |
| `service-worker.js` | Offline caching |
| `icon-192.svg` | App icon (192x192) |
| `icon-512.svg` | App icon (512x512) |

## Configuration

Settings stored in localStorage:

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `apiToken` | string | (empty) | Anthropic API key |
| `autoProcess` | boolean | true | Auto-process on paste |
| `forceLanguage` | boolean | false | Override input language |
| `outputLanguage` | string | (empty) | Target language when forced |

## Documentation

- [README.md](./docs/README.md) - User documentation
- [DEPLOY.md](./docs/DEPLOY.md) - Deployment guide

## Related Files

- [TODO.md](./TODO.md) - Task tracking
- [STATUS.md](./STATUS.md) - Current state
- [CODEBASE.md](./CODEBASE.md) - File structure
- [CHANGELOG.md](./CHANGELOG.md) - Version history
