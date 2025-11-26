# Lessons Learned

## PWA Development

### iOS PWA Quirks

**Problem:** iOS Safari has unique behaviors for PWAs that differ from Android.

**Lessons:**
- Always test on actual iOS devices, not just simulators
- iOS safe area insets need explicit handling (`env(safe-area-inset-*)`)
- Toast notifications should be at the top on iOS (bottom gets hidden)
- Keyboard handling differs - may need to disable editor keyboards

**Reference:** v1.1.0, v1.1.1 commits

### Manifest Path Issues

**Problem:** PWA manifest paths can be relative or absolute, causing issues on different hosts.

**Solution:** Use relative paths with `./` prefix:
```json
"start_url": "./index.html",
"scope": "./",
"icons": [{ "src": "./icon-192.svg", ... }]
```

**Reference:** v1.2.0 commit

### Service Worker Caching

**Problem:** API requests should NOT be cached by service worker.

**Solution:** Skip cache for API domain:
```javascript
if (event.request.url.includes('api.anthropic.com')) {
  return; // Don't cache API calls
}
```

## User Experience

### Hard Reset Behavior

**Problem:** "Hard reset" was clearing everything including API token, frustrating users.

**Solution:** Hard reset should preserve API token since it's tedious to re-enter.

**Reference:** v1.1.2 commit

### Settings Persistence

**Problem:** Users expected settings to persist across sessions.

**Solution:** Use localStorage for all settings:
- `apiToken`
- `autoProcess`
- `forceLanguage`
- `outputLanguage`

## API Integration

### Anthropic Browser Access

**Problem:** Anthropic API blocks direct browser requests by default.

**Solution:** Include special header:
```javascript
'anthropic-dangerous-direct-browser-access': 'true'
```

**Note:** This is intentionally named "dangerous" - users should understand their API key is in the browser.

## Naming

### App Rename Consideration

**Problem:** Renamed from "Touch Up" to "Clipot" but documentation got out of sync.

**Lesson:** When renaming, update ALL references:
- HTML title
- manifest.json (name, short_name)
- README.md
- All documentation
- Service worker cache name (if includes app name)
