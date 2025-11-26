# Project Workflow

## Development Workflow

### Local Development

```bash
# Start local server
python3 -m http.server 8000

# Or with Node.js
npx http-server -p 8000

# Visit http://localhost:8000
```

### Making Changes

1. Edit `index.html` (all code is in this single file)
2. Refresh browser to test
3. Test on mobile (iOS Safari, Android Chrome)
4. Commit and push to deploy

### Deployment

The app auto-deploys on git push to:
- **GitHub Pages** (primary)
- Netlify, Vercel, Cloudflare (alternatives)

See `docs/DEPLOY.md` for detailed instructions.

## AI Assistant Workflow

### Starting a Session

1. Read `.context/project/INDEX.md` for project overview
2. Check `main/STATUS.md` for current state
3. Review `main/TODO.md` for pending tasks
4. Review `LESSONS.md` to avoid repeating mistakes

### During Development

1. Update `main/TODO.md` as tasks progress
2. Document decisions in `main/CHANGELOG.md`
3. Keep `main/STATUS.md` current

### Session End

1. Update `main/STATUS.md` with current state
2. Add any lessons learned to `LESSONS.md`
3. Document breaking changes in `main/CHANGELOG.md`

## File Organization

```
.context/project/
├── INDEX.md          # Project overview (start here)
├── WORKFLOW.md       # This file
├── PRINCIPLES.md     # Design principles
├── LESSONS.md        # Lessons learned
├── main/             # Core application
│   ├── INDEX.md      # Subproject overview
│   ├── TODO.md       # Task tracking
│   ├── STATUS.md     # Current state
│   ├── CODEBASE.md   # File structure
│   ├── CHANGELOG.md  # Version history
│   └── docs/         # Documentation
│       ├── README.md
│       └── DEPLOY.md
└── archive/          # Old/deprecated content
```

## Branching Strategy

- `main` - Production branch, auto-deploys
- Feature branches as needed for larger changes

## Version Numbering

Using semantic versioning: `vMAJOR.MINOR.PATCH`

- MAJOR: Breaking changes
- MINOR: New features
- PATCH: Bug fixes
