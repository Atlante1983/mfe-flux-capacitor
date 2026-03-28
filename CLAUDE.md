# MFE Flux Capacitor — Tech Connect

## Project Overview
Interactive web visualization of the **Media For Europe (MFE)** corporate structure. Single-page HTML app (`index.html`) displaying hierarchical relationships between MFE and its European media subsidiaries.

## Architecture
- **Single file**: All logic in `index.html` (~735 lines) — HTML + CSS + vanilla JS
- **No build system, no frameworks, no package.json**
- **External deps**: Google Fonts (Outfit, Space Mono), html2canvas (CDN)
- **Language**: Spanish UI (`lang="es"`)

## Data Structure
Three regional branches defined in the `branches` array:
- **Mediaset Espana** (`es`) — color `#c0392b` (red), 17 subsidiaries, angle 210deg
- **ProSiebenSat.1** (`de`) — color `#34495e` (dark blue), 13 subsidiaries, angle 90deg
- **Mediaset Italia** (`it`) — color `#16a085` (teal), 17 subsidiaries, angle 330deg

Each subsidiary has: `n` (name), `def` (default visible), `img` (logo path), `active` (runtime state), `uid` (branch+index).

## Visual Layers (z-index order)
1. **body::before** (z:0) — Animated gradient background
2. **body::after** (z:0) — Grid overlay
3. **#bgCanvas** (z:1) — Star field animation
4. **#energyCanvas** (z:2) — Energy particles + core glow
5. **#diagram > #world** (z:3) — Main node graph (SVG tubes + DOM nodes)
6. **Core node** (z:20) — "TECH CONNECT / Media For Europe" center circle
7. **Hub nodes** (z:15) — Country hub circles (210px)
8. **Leaf nodes** (z:10) — Subsidiary circles (106px)

## Key Systems
- **Zoom/Pan**: CSS transform on `#world`, mouse drag + wheel, scale 0.3-3.0, default 0.82
- **SVG Tubes**: Lines connecting core->hubs->leaves, togglable via `doLines`
- **Energy Particles**: Canvas-based flowing dots along tubes, togglable via `doAnim` (off by default)
- **Floating Animation**: Organic sinusoidal motion on all nodes when `doAnim` is true
- **PNG Export**: html2canvas at 3x scale, transparent background, disables all animations during capture via `.export-mode` CSS class

## Asset Directories
```
MEDIASET ESPANA/       — Spanish channel logos (+ BASES/ subfolder)
MEDIASET ITALIA/       — Italian channel logos (+ BASES/ subfolder)
MFE/                   — Corporate logos (+ BASES/ subfolder)
PROSIEBENSAT.1 /       — German channel logos (+ BASES/ subfolder, note trailing space in dir name)
```

## Important Quirks
- `PROSIEBENSAT.1 /` directory has a **trailing space** before the slash
- Coordinates use center of viewport (`CX = W/2`, `CY = H*0.50`)
- Hub distance: `BD = min(W,H) * 0.32`; leaf distance: `LR_base = BD * 0.72`
- Energy particle screen coords account for zoom/pan transform offset
- Export resets zoom to 0.75 scale, waits 600ms for CSS transitions to settle

## Git Conventions
- Author: Santiago Romero
- Commit style: descriptive, present tense ("Fix...", "Add...", "Enlarge...")
- Branch: `main`
