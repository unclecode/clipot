# Touch Up - PWA

Polish your thoughts with AI. A simple, elegant Progressive Web App for text refinement.

## Features

- **Auto-process on paste**: Automatically refines text when you paste
- **Language support**: Maintains input language or forces specific output language
- **Offline capable**: Works offline after first load
- **Installable**: Add to home screen on iOS/Android

## Installation on iOS

1. Open Safari and navigate to your app URL (e.g., `http://localhost:8000`)
2. Tap the Share button (square with arrow pointing up)
3. Scroll down and tap "Add to Home Screen"
4. Tap "Add" in the top right corner
5. The app will appear on your home screen

## Setup

1. Open the app
2. Tap the settings icon (gear) in the top right
3. Enter your Anthropic API token
4. Configure your preferences:
   - Auto-process on paste
   - Force output language (optional)
   - Select output language

## Usage

1. Tap the clipboard icon or paste text
2. The app will automatically process (if enabled) or click "Touch Up"
3. Click "Try Again" for different variations
4. Text is automatically copied to clipboard

## Settings

- **API Token**: Your Anthropic API key (required)
- **Auto-process on paste**: Process immediately after pasting (default: on)
- **Force output language**: Override input language (default: off)
- **Output Language**: Target language when force is enabled

## Files

- `index.html` - Main app file
- `manifest.json` - PWA manifest
- `service-worker.js` - Service worker for offline support
- `icon-192.svg` - App icon (192x192)
- `icon-512.svg` - App icon (512x512)

## Development

```bash
# Start server
python3 -m http.server 8000

# Or use Node.js
npx http-server -p 8000
```

Visit `http://localhost:8000` in your browser.

## Privacy

- All data is stored locally in your browser
- API calls go directly to Anthropic (no intermediary server)
- No analytics or tracking

## License

MIT
