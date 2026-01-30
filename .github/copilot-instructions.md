# Kaladrishti - AI Agent Development Guide

## Project Overview

**Kaladrishti** is a web-based AI learning platform for classical Indian dance using MediaPipe for real-time pose detection. It combines sacred dance heritage with modern computer vision, providing frame-by-frame feedback through a browser-based UI built with Tailwind CSS and vanilla JavaScript.

The app is **frontend-only** (no backend server required) - all pose analysis, recording, and scoring happens in the browser using MediaPipe Holistic.

---

## Architecture & Data Flow

### Core Components

1. **View Router** (`index.html` - lines 400-450)
   - Single-page app with 5 main views: `landing` → `entry` → `selection` → `dashboard` → `lecture`
   - State-driven with `router.navigate(viewId)` function
   - Views hidden/shown via `.active` class on `.view-section` divs

2. **MediaPipe Holistic Integration** (`index.html` - lines 500-700)
   - Initialized in `app.initHolistic()` - returns `Holistic` object with pose/hand/face landmarks
   - Detects 33 body landmarks per frame
   - **Critical**: Use `holistic.onResults(callback)` to process results, not polling
   - Callback receives `results` object with: `poseLandmarks`, `leftHandLandmarks`, `rightHandLandmarks`, `faceLandmarks`

3. **State Management** (`index.html` - line 440)
   - Single `state` object tracks: current view, selected dance, active lecture, recording status, current score
   - Mode: `'camera'` (live video) or `'image'` (static image upload)
   - Update state directly; UI reads from state via functions like `app.analyzePosture()`

4. **Recording & Storage** (`index.html` - lines 1100-1150)
   - Uses `MediaRecorder` API to capture video stream
   - Stores session data in localStorage via `MongoService` class (lines 900-920)
   - Each session includes: dance name, lecture title, duration, score, verification status

### Data Flows

**Lecture Practice Flow:**
```
User selects dance → selectDance() → loads LECTURES → selectLecture() 
→ switchTab('practice') → router.navigate('lecture') → startCamera() 
→ MediaRecorder initialized → toggleRecording() records video 
→ MediaPipe sends pose data → analyzePosture() calculates score 
→ submitToDatabase() → API mock returns verification status → renderResults()
```

**Pose Analysis Flow:**
```
MediaPipe frame → onResults() → analyzePosture(poseLandmarks) 
→ Calculate spine/shoulder/arm alignment scores (40/40/20 weighting)
→ Update UI: posture bar, status color, feedback message
```

---

## Key Patterns & Conventions

### 1. **Glass Morphism UI**
- All cards use `.glass-card` class (lines 65-75)
- Combines: `backdrop-filter: blur(20px)`, semi-transparent bg, subtle borders
- Color tokens in CSS `:root` (lines 41-49): `--color-primary` (gold), `--color-accent` (red)
- Example: `<div class="glass-card p-6 rounded-2xl">`

### 2. **Pose Scoring Algorithm** (`analyzePosture()` lines 1020-1045)
- **Spine alignment**: Compare nose X-coordinate to center hip (40% weight)
- **Shoulder level**: Y-distance between shoulders (40% weight)
- **Arm spread**: Horizontal distance between elbows (20% weight)
- Final score: `Math.floor((spineScore * 0.4) + (shoulderScore * 0.4) + (armScore * 0.2))`
- Returns 0-100; colors code: >75 green (cyan), <75 red (#ff3333)

### 3. **Expression Detection** (`analyzeExpression()` lines 1000-1020)
- Face landmarks indices: mouth open (13, 14), smile width (61, 291), brow distance (107, 159, 336, 386)
- Thresholds: mouth open >0.08 = "Surprise", smile >0.5 = "Joy", low brow = "Anger"
- Updates UI bar colors: yellow/green/red/purple per emotion

### 4. **Event Dispatch Pattern**
- Pose detection events sent via `window.dispatchEvent(new CustomEvent('poseDetected', {detail: ...}))`
- Listened by UI components that need live updates
- Loose coupling: new modules can listen without modifying core detection

### 5. **Camera Modes**
- **Camera mode** (`state.mode = 'camera'`): Live webcam feed with continuous frame processing
- **Image mode** (`state.mode = 'image'`): Static image upload, single analysis
- Toggle in `startCamera()` vs `handleImageUpload()` (lines 920-960)

---

## Developer Workflows

### Running Locally
```powershell
# Start Python HTTP server
python -m http.server 8000

# Open http://localhost:8000 in Chrome/Firefox
```

### Adding a New Dance Form
1. Add entry to `DANCES` array (line 930): `{ id, name, img, origin }`
2. Add image to `media/img/` folder
3. Add lectures to `LECTURES` array (line 940)
4. Grid auto-renders from `DANCES` in `renderDances()` (line 950)

### Adding a New Pose Analysis Feature
1. Add landmark detection logic in `analyzePosture()` or `analyzeExpression()`
2. Update UI bars in `onResults()` (line 1015)
3. Update feedback message in `document.getElementById('ai-feedback')`
4. Test with camera and image uploads

### Debugging Pose Landmarks
- Toggle landmark visibility with button (lines 810-825): `app.toggleLandmarks()`
- When `state.showLandmarks = true`, draws all 33 body points + hand/face landmarks
- Landmark colors: gold for joints, cyan/red for feedback

---

## Critical Integration Points

### MediaPipe CDN URLs
- **Must use versioned HTTPS CDN** (lines 14-17): `@mediapipe/holistic` etc.
- Fallback to `/node_modules/@mediapipe` locally if npm installed
- Model files auto-downloaded on first `holistic.send()` call (~10-15 MB)

### LocalStorage Persistence
- `MongoService` class (lines 900-920) wraps `localStorage`
- Key format: `mongodb_<collectionName>` → JSON array
- Survives page refresh; cleared on browser cache clear
- Limit: ~5-10 MB per domain

### API Mock Integration
- `KaladrishtiAPI` class (lines 875-900) returns hardcoded responses
- Real backend should replace `sendAnalysisData()` logic
- Mock delay: 2000ms; production should verify actual pose accuracy server-side

---

## Common Pitfalls & Solutions

| Issue | Root Cause | Fix |
|-------|-----------|-----|
| Landmarks not visible | `state.showLandmarks = false` | Click eye icon or check toggle state |
| Camera won't start | Missing permissions or already in use | Check browser permissions, close other video apps |
| Video records but no analytics | `state.mode` not set correctly | Ensure `startCamera()` called before recording |
| Pose score stuck at 0 | Holistic not initialized | Wait for `onResults()` to fire at least once |
| Image upload stays loading | File type not supported | Use `.jpg`, `.png`; animated GIFs not supported |

---

## File Reference Guide

| File | Purpose | Key Functions |
|------|---------|---|
| [index.html](index.html) | Main app + embedded JS | `router.navigate()`, `app.selectDance()`, `analyzePosture()` |
| [src/app.js](src/app.js) | *Old codebase* - not used in current build | — |
| [src/utils/poseDetection.js](src/utils/poseDetection.js) | *Old codebase* - not used | — |
| [src/data/danceData.js](src/data/danceData.js) | *Old codebase* - data now in index.html DANCES/LECTURES arrays | — |
| [media/img/](media/img/) | Dance form images (PNG) | Referenced in `DANCES` array |
| [media/vid/dancer-bg.mp4](media/vid/) | Tutorial video for practice tab | Autoplay loop |
| [package.json](package.json) | Dependencies: @mediapipe/holistic, drawing_utils, camera_utils | `npm install` if needed |

---

## For AI Agents: High-Priority Tasks

When modifying this codebase:

1. **Always preserve** `state` object structure - it's the single source of truth
2. **Test pose detection** with both camera and image uploads
3. **Update localStorage keys** if changing `MongoService` collection names
4. **Verify Tailwind classes** exist; use existing color tokens (`--color-primary`, `--color-accent`)
5. **Check mirror-mode flag** for correct canvas orientation (camera vs image)
6. **Validate MediaPipe landmark indices** when adding new body part analysis
7. **Document new pose metrics** in the scoring algorithm with rationale for weight percentages
