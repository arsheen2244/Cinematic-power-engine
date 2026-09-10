# Cinematic Fire Engine 🔥❄️

A real-time, webcam-driven elemental VFX playground. Raise your hands, and the browser tracks them frame-by-frame to conjure fire, ice, lightning, or colored energy that follows your gestures — pinch to draw, open your palm to charge power.

Built entirely in the browser with [MediaPipe Hands](https://developers.google.com/mediapipe/solutions/vision/hand_landmarker) for tracking and HTML5 Canvas for rendering — no backend, no build step, no dependencies to install.

![status](https://img.shields.io/badge/status-active-brightgreen) ![type](https://img.shields.io/badge/type-single--file%20web%20app-blue) ![license](https://img.shields.io/badge/license-MIT-lightgrey)

---

## ✨ Features

- **Two-handed independent tracking** — each hand drives its own element, so you can mix fire in one hand and ice in the other simultaneously.
- **Three power modes**, switchable live via on-screen buttons or keyboard shortcuts:
  | Mode | Key | Left hand | Right hand |
  |---|---|---|---|
  | Fire / Ice | `1` | 🔥 Fire | ❄️ Ice |
  | Lightning | `2` | ⚡ Charges with either open hand | ⚡ Charges with either open hand |
  | Red / Blue | `3` | 🔴 Red energy | 🔵 Blue energy |
- **Gesture-driven intensity** — open your hand to build up a glowing aura; the longer it's open, the stronger the effect. A sudden open triggers a "surge" burst.
- **Pinch-to-draw** — bring thumb and index finger together to paint persistent streaks of fire, ice, red, or blue energy in the air.
- **Procedural lightning bolts** that jitter and arc between your fingers and joints when in Lightning mode.
- **Cinematic dark-mode compositing** — a multiply-blended vignette plus additive (`lighter`) particle blending gives the whole scene a moody, glowing look rather than a flat webcam overlay.
- **Graceful failure states** — clear on-screen messaging (and a retry button) for denied camera permissions, missing cameras, unsupported browsers, or a failed model load, instead of a silent blank screen.
- **Live status + on-screen controls** — no dev console required to know what the app is doing or to switch modes on a touchscreen.

---

## 🚀 Getting started

No installation, no bundler, no `npm install`. This is a single self-contained HTML file.

1. Download `cinematic-fire-engine.html`.
2. Open it in a modern desktop or mobile browser (Chrome, Edge, or Firefox recommended).
3. Allow camera access when prompted.
4. Hold your hand(s) up to the camera and start experimenting.

> **Note:** Because this uses `getUserMedia`, most browsers require the page to be served over `https://` or opened from `localhost` — opening the raw file directly (`file://`) works in some browsers but may be blocked in others. If you hit issues, serve it locally instead:
> ```bash
> python3 -m http.server 8000
> # then open http://localhost:8000/cinematic-fire-engine.html
> ```

---

## 🖐️ Gesture guide

| Gesture | Effect |
|---|---|
| **Open hand** | Charges intensity for that hand's element. Hold it open to sustain the glow. |
| **Quick open (from closed)** | Triggers a one-off "surge" — a stronger burst of particles. |
| **Pinch (thumb + index touching)** | Draws a persistent streak of your element wherever your fingers move. Works in Fire/Ice and Red/Blue modes. |
| **Open hand while in Lightning mode** | Builds lightning intensity; bolts arc from your fingertips and knuckles. |
| **Move hand quickly** | Temporarily cancels pinch detection (prevents false triggers while gesturing). |

---

## 🏗️ How it works

```
Webcam feed
   │
   ▼
MediaPipe Hands  ──►  21 hand landmarks per detected hand (up to 2)
   │
   ▼
Gesture layer     ──►  isHandOpen() / isPinching()  → per-hand intensity, surge, pinch state
   │
   ▼
Style layer        ──►  POWER_STYLES config maps (mode, hand) → element type + glow colors
   │
   ▼
Render layer        ──►  renderHandGlow() + spawnParticle() + spawnLightning()
   │
   ▼
Canvas composite     ──►  multiply vignette → additive particles/bolts → screen-blended glow
```

**Key design decisions:**
- **Config-driven, not hardcoded.** All tunable thresholds (pinch sensitivity, glow radius, decay rates, intensity rise/fall) live in a single `CONFIG` object at the top of the script — adjust behavior without touching logic.
- **One particle function, not four.** Fire, ice, red, and blue particles are all produced by a single `spawnParticle(type, x, y, isDrawing)` call, colored via a `PARTICLE_COLORS` lookup table — adding a new element is a config entry, not a new function.
- **One glow renderer, not four.** `renderHandGlow()` takes a hand index and a pair of colors, so Fire/Ice and Red/Blue modes share the exact same rendering path.
- **Mirrored view.** The canvas is flipped (`transform: scaleX(-1)`) so the experience feels like a mirror rather than a rear camera — standard for gesture-based interfaces.

---

## 🛠️ Tech stack

- **[MediaPipe Hands](https://developers.google.com/mediapipe/solutions/vision/hand_landmarker)** — pretrained hand landmark detection (21 points per hand, up to 2 hands, loaded via CDN)
- **HTML5 Canvas 2D API** — all rendering: radial gradients, additive/multiply/screen compositing, procedural particle and lightning paths
- **Vanilla JavaScript (ES6+)** — no framework, no build tooling
- **`getUserMedia` / MediaPipe Camera Utils** — webcam capture and frame feeding

---

## ⚠️ Known limitations

- Particle count is currently uncapped — sustained high-intensity effects with both hands over a long session may impact frame rate on lower-end devices.
- Hand-tracking accuracy depends on lighting and camera quality; poor lighting can cause flickering intensity.
- Everything runs in a single file/script for simplicity — a larger feature set (recording, more powers, multiplayer) would benefit from splitting into modules (gesture detection, particle system, render layer, UI).
- No automated tests; gesture behavior has been validated manually.

---

## 🗺️ Roadmap ideas

- [ ] Cap max active particles and auto-scale effect density to maintain frame rate
- [ ] Record/export a short clip of a session as a video or GIF
- [ ] Add a "combine" mode where both hands contribute to one merged effect
- [ ] Mobile-specific UI polish (larger touch targets, orientation handling)
- [ ] Split into ES modules (`gestures.js`, `particles.js`, `render.js`) for maintainability

---

## 📄 License

MIT — use it, fork it, remix it.

---

## 🙋 About

Built as an exploration of real-time computer vision + creative coding in the browser. Feedback and pull requests welcome.
