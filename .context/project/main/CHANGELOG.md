# CHANGELOG - main

All notable changes to the Clipot PWA.

## [v1.2.0] - 2025-10-24

### Changed
- Renamed application from "Touch Up" to "Clipot"
- Updated manifest.json with new name
- Fixed manifest paths to use relative URLs (`./` prefix)

### Fixed
- Manifest path issues on different hosting environments

---

## [v1.1.2] - 2025-10-24

### Fixed
- Hard reset now preserves API token (was clearing everything)

---

## [v1.1.1] - 2025-10-24

### Changed
- Moved toast notifications to top of screen (bottom was hidden on some devices)

---

## [v1.1.0] - 2025-10-24

### Added
- Clipboard icon button for quick paste access
- Proper iOS safe area support (notch/home indicator)

### Fixed
- iOS safe area insets now properly handled
- Disabled editor keyboard for better mobile UX

---

## [v1.0.0] - 2025-10-24

### Added
- Initial release
- Text refinement via Anthropic Claude API
- Auto-process on paste functionality
- Manual "Touch Up" button
- "Try Again" for variations
- Auto-copy results to clipboard
- Settings panel:
  - API token configuration
  - Auto-process toggle
  - Force output language option
  - Language selection
- PWA features:
  - Web App Manifest
  - Service Worker for offline support
  - iOS/Android installation support
- Dark theme UI
- Mobile-optimized layout (430px max-width)

---

## Version History Summary

| Version | Date | Type | Summary |
|---------|------|------|---------|
| v1.2.0 | 2025-10-24 | Feature | App rename to Clipot, manifest fixes |
| v1.1.2 | 2025-10-24 | Fix | Hard reset preserves token |
| v1.1.1 | 2025-10-24 | Fix | Toast position |
| v1.1.0 | 2025-10-24 | Feature | iOS fixes, clipboard icon |
| v1.0.0 | 2025-10-24 | Initial | First release |
