# Grammar Touch-Up - macOS System-Wide Grammar Correction

A macOS automation that allows grammar correction via keyboard shortcut in any application. Select text, press shortcut, get refined text.

**Created:** 2024-11-28
**Last Updated:** 2024-11-28

---

## Quick Reference

| Item | Path |
|------|------|
| Config (API Key) | `~/.grammar-touchup-config` |
| Shell Script | `~/Library/Scripts/GrammarTouchUp/grammar-touchup.sh` |
| Automator Workflow | `~/Library/Services/GrammarTouchUp.workflow/` |
| AppleScript (embedded) | `~/Library/Services/GrammarTouchUp.workflow/Contents/document.wflow` |
| Keyboard Shortcut | `⌃⇧⌘P` (Control+Shift+Command+P) |

---

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                     User presses ⌃⇧⌘P                           │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│              macOS Services Menu / Keyboard Shortcut            │
│                    (System Settings binding)                     │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                 Automator Quick Action                          │
│         ~/Library/Services/GrammarTouchUp.workflow/             │
│                                                                 │
│  1. Play start sound (Pop)                                      │
│  2. Save clipboard                                              │
│  3. Copy selected text (Cmd+C)                                  │
│  4. Call shell script with text                                 │
│  5. Paste result (Cmd+V)                                        │
│  6. Restore clipboard                                           │
│  7. Play done sound (Glass) or error sound (Basso)              │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      Shell Script                               │
│       ~/Library/Scripts/GrammarTouchUp/grammar-touchup.sh       │
│                                                                 │
│  1. Load API key from ~/.grammar-touchup-config                 │
│  2. Build request with system prompt                            │
│  3. Call Anthropic API                                          │
│  4. Parse JSON response                                         │
│  5. Return refined text                                         │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Anthropic API                                │
│              Model: claude-haiku-4-5-20251001                   │
│              Endpoint: api.anthropic.com/v1/messages            │
└─────────────────────────────────────────────────────────────────┘
```

---

## File Details

### 1. Config File: `~/.grammar-touchup-config`

Simple key-value file sourced by the shell script.

```bash
ANTHROPIC_API_KEY=sk-ant-api03-xxxxx
```

**To view:**
```bash
cat ~/.grammar-touchup-config
```

**To edit:**
```bash
nano ~/.grammar-touchup-config
```

---

### 2. Shell Script: `~/Library/Scripts/GrammarTouchUp/grammar-touchup.sh`

The main script that:
- Loads API key
- Contains the full system prompt (Tom's Grammar & Rewrite Style Agent)
- Makes the API call using `curl`
- Parses response using `jq`

**To view:**
```bash
cat ~/Library/Scripts/GrammarTouchUp/grammar-touchup.sh
```

**To edit:**
```bash
nano ~/Library/Scripts/GrammarTouchUp/grammar-touchup.sh
```

**To test directly:**
```bash
~/Library/Scripts/GrammarTouchUp/grammar-touchup.sh "I has went to store"
```

**Dependencies:**
- `jq` (JSON parser) - install via `brew install jq`
- `curl` (pre-installed on macOS)

---

### 3. Automator Workflow: `~/Library/Services/GrammarTouchUp.workflow/`

Structure:
```
GrammarTouchUp.workflow/
└── Contents/
    ├── Info.plist        # Service metadata (name, menu entry)
    └── document.wflow    # Contains the AppleScript (XML format)
```

The AppleScript is embedded in `document.wflow` as XML. Key sections:
- Sound effects using `afplay`
- Clipboard management
- Keystroke simulation via System Events

**To view the AppleScript:**
```bash
cat ~/Library/Services/GrammarTouchUp.workflow/Contents/document.wflow
```

**To edit:** Edit the `document.wflow` file directly (it's XML with embedded AppleScript in the `<string>` tag under `ActionParameters > source`).

---

## Model Configuration

| Setting | Value |
|---------|-------|
| Provider | Anthropic |
| Model | `claude-haiku-4-5-20251001` |
| Max Tokens | 2000 |
| API Version | `2023-06-01` |

**System Prompt:** Tom's Grammar & Rewrite Style Agent
- Light Mode: Grammar touch-up (minimal changes)
- Deep Mode: Rewrite/edit (more aggressive)
- Default: Light Mode
- Preferences: No em-dashes, conversational tone, contractions

**Response Format:** JSON
```json
{"response": "refined text here"}
```

---

## Sound Effects

| Event | Sound | Path |
|-------|-------|------|
| Start | Pop | `/System/Library/Sounds/Pop.aiff` |
| Success | Glass | `/System/Library/Sounds/Glass.aiff` |
| Error | Basso | `/System/Library/Sounds/Basso.aiff` |

**Available system sounds:**
```bash
ls /System/Library/Sounds/
```

---

## Accessibility Permissions

These apps must have Accessibility permission (System Settings → Privacy & Security → Accessibility):

1. `/System/Applications/Automator.app`
2. `/System/Applications/Utilities/Script Editor.app`
3. `/System/Library/CoreServices/System Events.app`
4. Any app where you use the shortcut (Chrome, Slack, etc.)

**If shortcuts stop working:**
1. Toggle permissions OFF for all these apps
2. Close System Settings
3. Reopen and toggle permissions ON
4. In Automator: Menu → Automator → Reset Warnings

---

## Keyboard Shortcut Setup

Location: System Settings → Keyboard → Keyboard Shortcuts → Services → General

| Service Name | Shortcut |
|--------------|----------|
| GrammarTouchUp | ⌃⇧⌘P |

**Important:** Service name must NOT have spaces (causes issues).

---

## Common Iterations

### Change the LLM Model

Edit `~/Library/Scripts/GrammarTouchUp/grammar-touchup.sh`, find:
```bash
model: "claude-haiku-4-5-20251001",
```

Replace with desired model (e.g., `claude-sonnet-4-20250514`).

### Change System Prompt

Edit `~/Library/Scripts/GrammarTouchUp/grammar-touchup.sh`, find the `SYSTEM_PROMPT` variable and modify.

### Change Sounds

Edit `~/Library/Services/GrammarTouchUp.workflow/Contents/document.wflow`, find lines like:
```
afplay /System/Library/Sounds/Pop.aiff
```

Replace with another sound from `/System/Library/Sounds/`.

### Change Keyboard Shortcut

System Settings → Keyboard → Keyboard Shortcuts → Services → General → GrammarTouchUp

### Add New Mode/Feature

1. Update system prompt in shell script
2. Optionally create a second workflow with different shortcut for different mode

---

## Troubleshooting

### "Not allowed to send keystrokes"
- Toggle Accessibility permissions off/on for Automator, Script Editor, System Events
- Run Automator → Menu → Reset Warnings

### No sound on start
- The 100-200ms delay is Automator service startup time (unavoidable)
- Sound plays immediately when script starts

### API errors
- Check API key: `cat ~/.grammar-touchup-config`
- Test script directly: `~/Library/Scripts/GrammarTouchUp/grammar-touchup.sh "test"`
- Check jq installed: `which jq`

### Service not appearing in Keyboard Shortcuts
- Refresh services: `/System/Library/CoreServices/pbs -update`
- Log out and back in

---

## Changelog

### 2024-11-28 - Initial Setup
- Created shell script with Anthropic API integration
- Created Automator Quick Action
- Added sound feedback (Pop/Glass/Basso)
- Set up keyboard shortcut ⌃⇧⌘P
- System prompt: Tom's Grammar & Rewrite Style Agent (from touch-up-grammar PWA)

---

## Related Projects

- **Touch-Up Grammar PWA:** `/Users/unclecode/devs/touch-up-grammar/`
  - System prompt source: `/Users/unclecode/devs/touch-up-grammar/system-prompt.md`
  - Same API configuration and prompt as this macOS automation

---

## Future Ideas

- [ ] Add second shortcut for "Rewrite Mode" (deep editing)
- [ ] Add option to choose output language
- [ ] Menu bar indicator while processing
- [ ] Support for OpenAI/Gemini as alternative providers
