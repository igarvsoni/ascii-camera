# ASCII Camera

**🌐 Live demo:** [igarvsoni.github.io/ascii-camera](https://igarvsoni.github.io/ascii-camera/)

### 📱 Open on mobile

Scan to launch on your phone:

<img src="qr-code.png" alt="QR code to ASCII Camera" width="200" />

## What Is This?

ASCII Camera is a browser-based tool that turns your webcam feed into real-time ASCII art. It has an IDE/code-editor inspired interface. Everything runs locally — no server, no uploads, no data leaves your machine.

## Features

### Live ASCII Preview
Your webcam feed is converted to ASCII characters in real-time with an FPS counter, line numbers, and a file-tab style preview.

### 8 Filters
| Filter | Effect |
|--------|--------|
| None | Raw webcam feed as ASCII |
| B&W | Pure grayscale character mapping |
| Retro | Warm sepia/amber tones |
| Inverted | Flipped brightness |
| Neon | Green terminal glow |
| Blueprint | Blue-tinted technical drawing look |
| Thermal | Heat map colors (blue → red) |
| Posterize | Quantized color bands |

### 6 Camera Controls
| Control | What It Does |
|---------|-------------|
| Brightness | Lighter or darker |
| Contrast | Difference between light and dark |
| Exposure | Simulated camera exposure |
| Saturation | Color intensity |
| Sharpness | Edge enhancement |
| Gamma | Tonal curve adjustment |

### ASCII Character Sets
- **Standard** — 10-character ramp
- **Detailed** — 70-character fine gradient
- **Blocks** — Unicode block characters (░▒▓█)
- **Simple** — Minimal 5-character set

### Export Options
- **PNG** — Shareable image with ASCII art rendered as text
- **JPG** — Compressed image export
- **TXT** — Raw ASCII text file
- Configurable image scale (1x–5x)
- Selectable font color or auto-match to filter

### Background Options
4 background color presets: Dark, Darker, Midnight, Light

## Tech Stack
- HTML5 / CSS3 / Vanilla JavaScript
- WebRTC (getUserMedia API)
- Canvas API (pixel processing + image export)
- Google Fonts (JetBrains Mono)

## Privacy
All processing happens locally in your browser. No images or data are sent anywhere.
