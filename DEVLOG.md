# Development Log — ASCII Camera

## What Was Built

A single-page ASCII camera website (`index.html`) with a code editor / IDE-inspired dark UI. Captures live webcam feed and renders it as real-time ASCII art with filters, camera controls, and image export (PNG/JPG/TXT).

## How It Works

### Core Architecture
Everything lives in one HTML file with embedded CSS and JavaScript. Uses JetBrains Mono font for a code-editor aesthetic. No frameworks, no build tools.

### The Rendering Pipeline
Each frame goes through this pipeline at ~30fps:

1. **Webcam capture** — `getUserMedia()` streams video into a hidden `<video>` element
2. **Canvas draw** — Each frame is drawn onto a hidden `<canvas>` (mirrored horizontally)
3. **Sharpening** (optional) — 5-point Laplacian kernel applied to pixel data
4. **Pixel sampling** — `getImageData()` reads RGBA values; pixels are grouped into blocks (size set by Resolution slider)
5. **Filter application** — Selected filter transforms RGB values
6. **Adjustment application** — Brightness, contrast, exposure, saturation, gamma applied mathematically
7. **ASCII mapping** — Luminance formula (`0.299R + 0.587G + 0.114B`) maps brightness to charset character
8. **Render** — ASCII string set as text content of a `<pre>` element with line numbers

### Filters (8 total)
- **B&W** — Weighted luminance grayscale
- **Retro/Sepia** — Sepia tone matrix multiplication
- **Inverted** — `255 - value` per channel
- **Neon/Matrix** — Green channel isolation + CSS glow
- **Blueprint** — Blue-tinted grayscale
- **Thermal** — Heat map gradient (blue → yellow → red)
- **Posterize** — 4-level color quantization

### Adjustments (6 sliders)
- **Brightness** — Addition to pixel values
- **Contrast** — Standard contrast formula
- **Exposure** — Pixel value multiplication
- **Saturation** — Grayscale-to-color interpolation
- **Sharpness** — Laplacian edge enhancement kernel
- **Gamma** — Power curve correction

### Image Export (PNG/JPG)
- ASCII text is rendered onto a hidden `<canvas>` using `fillText()`
- Configurable scale (1x–5x) controls output image resolution
- Font color selectable or auto-matched to current filter
- Canvas exported via `toBlob()` → download as PNG or JPG
- TXT export still available via Blob

### UI Design
- IDE/code-editor inspired layout with window dots, file tabs, line numbers
- Left sidebar with 3 tabs: Controls, Filters, Export
- Filter pills in a 2-column grid with color-coded active states
- FPS counter and live status indicator in header
- Captured frame panel at bottom with toggle
- Toast notifications (bottom-right)

## Design Decisions
- **Single file** — Maximum portability
- **IDE aesthetic** — Feels like code, not a typical camera app
- **Tab-based sidebar** — Keeps controls organized without overwhelming the UI
- **Canvas-based image export** — Renders ASCII as actual text on canvas for shareable PNG/JPG
- **Line numbers** — Reinforces the code-editor feel
- **`willReadFrequently: true`** — Optimizes canvas for repeated pixel reads
