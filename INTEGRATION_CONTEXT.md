# Transformers.js Integration - Context for Next Session

## PROJECT OVERVIEW
**App:** Clipot PWA - Text refinement tool for iPhone 14 Pro Max
**Location:** `/Users/unclecode/devs/touch-up-grammar/index.html` (single-file app)
**Goal:** Add local on-device text processing using Transformers.js as alternative to Anthropic API

## WHAT WAS ATTEMPTED

### Initial Plan (Correct)
1. Add Transformers.js CDN import
2. Create UI: toggle + model selector + download button + progress overlay
3. User selects model → clicks download → overlay shows progress → model ready
4. When processing: if model ready → use local, else → error message
5. No auto-download, explicit download only

### What Got Implemented (BROKEN)

#### ✅ UI Components Added (Lines ~638-795):
- Toggle: "Use Local Model (iPhone)"
- Radio buttons (replaced dropdown):
  - `onnx-community/Qwen2.5-0.5B-Instruct` (200MB) - Recommended
  - `onnx-community/Phi-3.5-mini-instruct` (1.5GB) - Highest quality
  - `onnx-community/LFM2-350M-ONNX` (140MB) - Fast
- Model status: `<span id="modelStatusText">Not Downloaded</span>`
- Download button: `<button id="downloadModelBtn">Download Model</button>`

#### ✅ Download Overlay Added (Lines ~822-835):
```html
<div class="download-overlay" id="downloadOverlay">
  <div class="download-content">
    <div class="download-title">Downloading Model</div>
    <div class="download-subtitle" id="downloadModelName"></div>
    <div class="progress-bar-container">
      <div class="progress-bar-fill" id="progressBarFill"></div>
    </div>
    <div class="progress-text" id="progressText">0%</div>
    <div class="progress-details" id="progressDetails"></div>
  </div>
</div>
```

#### ✅ CSS Added (Lines ~546-668):
- `.model-option` - Radio button styling
- `.model-status` - Status display
- `.download-overlay` - Full-screen overlay
- `.progress-bar-container` + `.progress-bar-fill` - Progress bar

#### ✅ Transformers.js CDN Import (Lines ~1285-1296):
```html
<script type="module">
  import { pipeline, env } from 'https://cdn.jsdelivr.net/npm/@huggingface/transformers@3.8.0';
  env.allowLocalModels = false;
  env.allowRemoteModels = true;
  env.backends.onnx.wasm.numThreads = 4;
  window.transformersPipeline = pipeline;
  window.transformersEnv = env;
</script>
```

#### ⚠️ State Variables Added (Lines ~874-879):
```javascript
let useLocalModel = false;
let selectedModel = 'onnx-community/Qwen2.5-0.5B-Instruct';
let localPipeline = null;
let modelLoading = false;
let modelLoadProgress = 0;
let modelReady = false;
```

#### ⚠️ Functions Added:

**downloadModel()** (Lines ~940-1041):
- Shows overlay with progress bar
- Progress simulation: 0% → 5% → 10% → ... → 90% (every 2s)
- Loads model: `window.transformersPipeline('text-generation', selectedModel, {quantized: true})`
- On success: Sets `modelReady = true`, updates status to "Ready ✓"
- On error: Shows error, updates status to "Download Failed"

**processWithLocalModel(text)** (Lines ~1043-1093):
- Checks if `localPipeline` exists
- Builds prompt: `${systemPrompt}\n\nUSER: ${text}\n\nASSISTANT: Here is the refined text:`
- Calls: `await localPipeline(prompt, {max_new_tokens: 512, temperature: 0.7, ...})`
- Cleans output: removes prompt echo, removes "Here is the refined text:" prefix
- Returns cleaned text

**touchUp() modification** (Lines ~1146-1243):
- Check 1: If `useLocalModel` is ON
- Check 2: If model not ready: `showStatus('Please download the model first')`
- Check 3: Call `processWithLocalModel(textToProcess)`
- If result is null: fallback to API
- If success: Show "✓ Touched up (Local)", copy to clipboard

#### ⚠️ Event Handlers Added:

**Toggle handler** (Lines ~1532-1553):
- Shows/hides model selection container
- Clears pipeline if disabling

**Radio button handlers** (Lines ~1555-1574):
- Updates `selectedModel` on change
- Clears pipeline and resets status if switching models

**Download button handler** (Lines ~1576-1580):
```javascript
downloadModelBtn.addEventListener('click', async () => {
  console.log('Download button clicked');
  await downloadModel();
});
```

## WHERE IT BROKE (CRITICAL ISSUES)

### 🔴 Issue 1: DOM Element Mismatch
**Problem:** Code references `localModelSelect` (dropdown) but HTML has radio buttons `input[name="localModel"]`
**Impact:** Event handlers may not work
**Fix needed:** Ensure all DOM references match actual HTML elements

### 🔴 Issue 2: Settings Button Not Working
**Symptom:** User reports clicking settings does nothing
**Likely cause:** JavaScript error breaking execution before event handlers attach
**Check:** Console errors, ensure all `getElementById()` calls find elements

### 🔴 Issue 3: Paste Button Not Working
**Symptom:** Paste button unresponsive
**Likely cause:** Same JS error blocking all event handlers
**Location:** `pasteBtn.addEventListener('click', pasteFromClipboard)` - may not be reached

### 🔴 Issue 4: Initial Auto-Download Bug (FIXED BUT CREATED MESS)
**Original problem:** Model tried to download on paste automatically
**Root cause:** `touchUp()` called `processWithLocalModel()` → called old `loadLocalModel()` → auto-downloaded
**Fix attempt:** Removed auto-download, required explicit download button
**Side effect:** May have broken something in the process

## TRANSFORMERS.JS RESEARCH FINDINGS

### Library Info
- **Package:** `@huggingface/transformers` v3.8.0 (latest as of Nov 2024)
- **Formerly:** `@xenova/transformers` (renamed)
- **CDN:** `https://cdn.jsdelivr.net/npm/@huggingface/transformers@3.8.0`
- **Docs:** https://huggingface.co/docs/transformers.js

### How It Works
1. **Import:** `import { pipeline } from '@huggingface/transformers'`
2. **Create pipeline:** `pipeline('text-generation', 'model-name', options)`
3. **Run inference:** `await generator(prompt, generationOptions)`
4. **Caching:** Models cached in browser IndexedDB automatically
5. **Backends:** WebGPU (fast) or WASM (fallback) - iPhone uses WASM

### Recommended Models for Grammar/Text Refinement
**Selected for this app:**
1. **Qwen2.5-0.5B-Instruct** - 200MB, best balance quality/speed, good instruction-following
2. **Phi-3.5-mini-instruct** - 1.5GB, highest quality, slower
3. **LFM2-350M-ONNX** - 140MB, fastest, good quality

**Why NOT T5-grammar models:**
- T5 models are encoder-decoder, need different pipeline type ('text2text-generation')
- Instruction-following models (Qwen, Phi, LFM) better handle complex system prompts
- User specifically requested LFM2-350M be included

### Pipeline Options Used
```javascript
{
  quantized: true,  // Smaller model size, faster load
  progress_callback: (progress) => { /* log progress */ }
}
```

### Generation Options Used
```javascript
{
  max_new_tokens: 512,      // Max output length
  temperature: 0.7,         // Randomness (0=deterministic, 1=creative)
  do_sample: true,          // Enable sampling
  top_p: 0.9,              // Nucleus sampling
  repetition_penalty: 1.1   // Reduce repetition
}
```

### Expected Behavior
- **First download:** 1-3 minutes for 200MB on WiFi
- **Subsequent loads:** <3 seconds from IndexedDB cache
- **Inference time:** 2-4 seconds per text on iPhone 14 Pro Max
- **Fully offline** after first download

## CORRECT IMPLEMENTATION FLOW

### User Journey (What SHOULD Happen)
1. User opens settings → toggles "Use Local Model" ON
2. Model selection appears (3 radio buttons)
3. User selects model (e.g., Qwen2.5-0.5B)
4. User clicks "Download Model" button
5. Overlay appears with progress bar
6. Progress updates: 0% → 5% → ... → 100% (every 2 seconds)
7. Model downloads in background (1-3 min)
8. Overlay closes, status shows "Ready ✓" (green)
9. User closes settings
10. User pastes text → clicks "Touch Up"
11. Text processes locally (2-4 seconds)
12. Result shows with status "✓ Touched up (Local)"

### Error Handling
- **No model downloaded + local mode ON:** "Please download the model first"
- **Download fails:** "Model download failed: [error]", status shows "Download Failed" (red)
- **Processing fails:** Falls back to API automatically
- **No API token + local mode OFF:** "Please set your API token in settings"

## FILES TO CHECK

### Primary File
`/Users/unclecode/devs/touch-up-grammar/index.html` - Everything is here

### Key Line Ranges
- **HTML UI:** Lines 638-795 (model selection)
- **HTML Overlay:** Lines 822-835 (progress overlay)
- **CSS:** Lines 546-668 (new styles)
- **CDN Import:** Lines 1285-1296 (Transformers.js)
- **State:** Lines 874-879 (variables)
- **Functions:** Lines 940-1093 (download, process)
- **touchUp:** Lines 1146-1243 (routing logic)
- **Event Handlers:** Lines 1532-1580 (toggle, radio, download btn)

### Supporting Files
- `/Users/unclecode/devs/touch-up-grammar/system-prompt.md` - Tom's grammar agent instructions
- `/Users/unclecode/devs/touch-up-grammar/service-worker.js` - PWA cache (may need version bump)

## CRITICAL DEBUGGING STEPS

1. **Open browser console** - Check for JavaScript errors
2. **Check if these elements exist:**
   - `document.getElementById('settingsBtn')`
   - `document.getElementById('downloadModelBtn')`
   - `document.getElementById('modelStatusText')`
   - `document.querySelectorAll('input[name="localModel"]')`
3. **Verify event handlers attached:**
   - Add `console.log('Handler attached')` after each addEventListener
4. **Check execution order:**
   - Ensure CDN script loads before main script tries to use `window.transformersPipeline`
5. **Verify no syntax errors:**
   - Look for mismatched quotes, brackets, parentheses
   - Check for duplicate variable declarations

## WHAT TO DO NEXT

### Step 1: Fix Broken UI
- Hard refresh page
- Open console
- Find the JavaScript error that's preventing ANY buttons from working
- Fix that error first (likely syntax error or undefined variable)

### Step 2: Test Basic Functionality
- Settings button should open modal
- Toggle should show/hide model selection
- Radio buttons should be selectable
- Download button should exist and be clickable

### Step 3: Test Download Flow
- Click download button
- Overlay should appear
- Progress should update every 2 seconds
- Model should download (wait 1-3 minutes)
- Overlay should close, status should show "Ready ✓"

### Step 4: Test Processing
- Paste text
- Click "Touch Up"
- Should process with local model (2-4 seconds)
- Result should appear
- Status should show "✓ Touched up (Local)"

## REFERENCES
- Transformers.js docs: https://huggingface.co/docs/transformers.js
- Qwen2.5 models: https://huggingface.co/onnx-community/Qwen2.5-0.5B-Instruct
- Phi-3.5: https://huggingface.co/onnx-community/Phi-3.5-mini-instruct
- LFM2-350M: https://huggingface.co/onnx-community/LFM2-350M-ONNX
- Original plan: `/Users/unclecode/.claude/plans/noble-wobbling-muffin.md`

## LESSONS LEARNED
1. **NEVER auto-download on user action** - Always require explicit download button
2. **Always show progress visually** - Overlay with progress bar, not just toast messages
3. **Test mobile view properly** - Dropdowns suck on mobile, use radio buttons
4. **Check element existence** - Verify all getElementById() calls match actual HTML
5. **One change at a time** - Don't refactor everything at once
6. **Test after each step** - Would have caught the broken UI immediately

---

**STATUS:** BROKEN - Settings and paste buttons not working at all
**PRIORITY:** Fix the JavaScript error breaking ALL event handlers
**SERVER:** Running on http://localhost:8000
