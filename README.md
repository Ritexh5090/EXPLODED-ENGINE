# EXPLODED ENGINE

**Pull machines apart with your bare hands.**
A webcam-based 3D interface: your hands are tracked in real time, and you use them to rotate, explode, zoom, and pull individual parts out of a 26-part rocket engine made of thousands of dots.

Computer vision + 3D + JavaScript. No downloads, no build step, one HTML file.

> Design & developed by **Ritesh Jagtap**
> Insta - **ritexh404.exe**

---

## Features

- Live webcam background with a hand skeleton overlay
- **Rocket Engine** with 26 separate parts (injector, combustion chamber, turbopumps, nozzle, gimbal actuators and more)
- Two extra machines: **Turbofan Jet** and **Robot Arm**
- **Exploded view** that spreads parts along the main axis, with dashed assembly rails showing where each part belongs
- **Pinch a part to pull it out** on its own. Release far and it stays removed, release near and it snaps back
- Rotate, zoom, and switch machines using only hand gestures
- Part labels with leader lines, and hover highlighting when you point at a part
- Mouse and keyboard fallback if you have no camera

---

## Tech used

| Tool | Purpose |
|---|---|
| HTML + CSS | Page structure, layer stacking, styling |
| JavaScript (ES modules) | All the logic: gestures, explode, grabbing parts |
| [Three.js](https://threejs.org) (v0.160.0) | Draws the 3D machine as particles |
| [MediaPipe Hand Landmarker](https://developers.google.com/mediapipe) (tasks-vision 0.10.14) | Tracks 21 points per hand from the webcam |
| Canvas 2D | Draws the hand skeleton, rings, labels and rails |

Three.js and MediaPipe load from a CDN, so **you need an internet connection** the first time you run it.

---

## How to run

The webcam only works on `localhost` or `https`, so do **not** just double-click the file. Start a small local server first.

1. Put `index.html` in a folder (for example `exploded-engine`) and open that folder in your editor (Antigravity or VS Code).
2. Start a server with **one** of these:
   - `npx vite` (needs Node.js installed)
   - `python -m http.server 8000` (needs Python installed)
   - the **Live Server** extension, then click "Go Live"
3. Open the link it prints (for example `http://localhost:5173` or `http://localhost:8000`) in **Chrome**.
4. Click **Allow** when the browser asks for camera access.

Wait for the status line at the bottom to say **"Hand tracker ready"**.

---

## Controls

### Hands

| Action | Gesture |
|---|---|
| Pull out one part | Pinch **on a part** and move your hand |
| Put a part back | Release it **near** its place and it snaps in |
| Leave a part removed | Release it **far** from its place |
| Rotate | Pinch on **empty space** and move |
| Exploded view | Two open hands, move them apart (together to reassemble) |
| Zoom | Pinch with both hands, move them apart or together |
| Switch machine | Point your index finger at a bottom tab for 1 second |
| Highlight a part | Point at it with one open hand |

### Keyboard and mouse

| Key / Action | What it does |
|---|---|
| `1` `2` `3` | Switch machine |
| `E` | Toggle exploded view |
| `R` | Reinstall all parts and reset the view |
| Drag a part | Pull it out |
| Drag empty space | Rotate |
| Scroll | Zoom |

---

## Tips for good tracking

- Use good, even lighting. Light from the front works best.
- Keep your hands fully in the camera frame.
- Pinch clearly: touch your thumb tip to your index fingertip.
- Sit about an arm's length from the camera.

---

## Project structure

```
exploded-engine/
├── index.html   <- the whole project (HTML, CSS, JavaScript)
└── README.md
```

Inside `index.html` the script is split into numbered sections:

1. Helpers
2. Three.js setup
3. Point samplers (shapes made of dots)
4. The machines (`MODELS`)
5. Loading a machine
6. Hand tracking (MediaPipe)
7. State, picking and grabbing parts
8. Gesture logic
9. HUD drawing
10. Main loop, start and resize

---

## Customize it

Settings at the top of the script:

| Setting | What it changes |
|---|---|
| `DOT_SIZE` | Size of every dot |
| `STRETCH` | How far parts spread along the axis when exploded |
| `RADIAL` | How far side parts slide out when exploded |
| `PULL_SNAP` | How close you must release a part for it to snap back |
| `HIT_RADIUS` | How close (in pixels) your pinch must be to grab a part |

### Add your own part

Find the machine in `MODELS` and add a line like this:

```js
P("Heat Shield", 0x8d99ae, torus(1.2, 0.05, 800), { pos: [0, 0.5, 0], delay: 0.1 }),
```

`P(name, color, points, options)`. Shapes available: `cyl`, `lathe`, `ball`, `torus`, `disc`, `blades`, `ribs`, `box`.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| Blank screen or error | Press `F12`, open the Console tab, and read the red error |
| Camera does not start | You are not on `localhost` or `https`, or you blocked camera access |
| "Hand tracker failed to load" | Check your internet connection (the model loads from a CDN) |
| Hands not detected | Improve lighting and keep both hands fully in frame |
| Wrong part grabbed | Lower `HIT_RADIUS`, or zoom in before pinching |
| Pinch is hard to trigger | Raise the pinch numbers in `readHands()` (`0.3` to start, `0.45` to hold) |

---

## What you learn from this project

- How a 3D scene works: scene, camera, renderer, and the animation loop
- Building shapes from math instead of downloading models
- How computer vision turns a camera image into numbers (21 hand points)
- Turning those numbers into gestures: pinch, distance, movement
- Converting between 2D screen positions and 3D space
- Smoothing, inertia, and staggered animation to make motion feel natural

---

## Credits

Design & developed by **Ritesh Jagtap**
Instagram: **ritexh404.exe**

Built with Three.js and Google MediaPipe.