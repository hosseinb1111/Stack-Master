# Stack Master 🏗️

A precision stacking game built with Three.js and GSAP. Drop blocks on top of each other as the tower rises — the closer you land to the center, the more you keep. Miss by too much, and it's game over.

The entire game ships as a **single HTML file** — no build step, no dependencies to install. Just open it in a browser.

---

## 🎮 How to Play

1. A block slides back and forth across the current platform.
2. **Tap, click, or press Space** to drop it.
3. The overlapping part stays; anything hanging off the edge gets sliced away and falls.
4. **Land close to center for a "perfect" bonus** — the block keeps its full size and stays perfectly aligned.
5. Each block shrinks a little if you're off-center. Once there's nothing left to land on, the game ends.
6. Your score = number of blocks in the tower.

The sliding axis alternates every block — X, then Z, then X, and so on — so you have to track both directions.

---

## ✨ Features

- **Smooth 3D visuals** — Toon-shaded blocks with an orthographic camera and dynamic camera follow.
- **Perfect-placement bonus** — Land within a small threshold and the block keeps its full width (no chop).
- **Physics-style slicing** — Misaligned drops split into a placed piece and a spinning, falling chopped piece.
- **Progressive difficulty** — Blocks move faster as the tower grows (capped so it stays playable).
- **Vibrant pastel palette** — Colors cycle procedurally using sine-wave offsets, so every run looks slightly different.
- **Score animation** — Counter tweens down through every block during the reset sequence.
- **Touch + mouse + keyboard** — Tap, click, or Space; the input handler de-duplicates simultaneous touch/click events.
- **Responsive layout** — Text scales down on narrow screens; canvas resizes on window/orientation change.
- **Graceful WebGL failure** — Shows a message if the context is lost instead of freezing silently.

---

## 🕹️ Controls

| Action | Input |
| --- | --- |
| Start game | Tap / Click / Space on the **START GAME** button |
| Drop block | Tap / Click anywhere / Space |
| Restart | Tap / Click / Space on the game-over screen |

All three input types trigger the same handler, with a 150 ms cooldown to prevent double-drops from a single tap.

---

## 🚀 Running It

**Option 1 — Just open it**
Double-click the `.html` file. That's it.

**Option 2 — Serve it locally** (recommended if your browser blocks CDN scripts from `file://`)

```bash
# Python 3
python -m http.server 8000

# Node
npx serve
```

Then visit `http://localhost:8000`.

---

## 🛠️ Tech Stack

| Library | Version | Purpose |
| --- | --- | --- |
| [Three.js](https://threejs.org/) | r83 | 3D rendering, geometry, lights, camera |
| [GSAP (TweenMax)](https://greensock.com/gsap/) | 1.18.0 | Camera tweening, chop-piece animation, score countdown |

Both are loaded from Cloudflare's CDN — no npm install required.

---

## ⚙️ Customization

A few knobs worth knowing if you want to tweak the feel:

| What | Where | Default |
| --- | --- | --- |
| Base block size | `Block` constructor → `dimension.width/depth` (first block) | `10 × 10` |
| Block height | `dimension.height` | `2` |
| Slide distance from center | `MOVE_AMOUNT` | `12` |
| Base movement speed | `rawSpeed = -0.1 - (index * 0.005)` | ramps per block |
| Max speed cap | `Math.max(rawSpeed, -4)` | `-4` |
| Perfect-placement threshold | `this.dimension[workingDim] - overlap < 0.3` | `0.3` units |
| Camera zoom / view size | `onResize()` → `viewSize` | `30` |
| Background color | `renderer.setClearColor('#D0CBC7')` | warm gray |
| Input cooldown | `withCooldown(fn, delayMs)` | `150 ms` |
| Instructions auto-hide | `if (this.blocks.length >= 5)` | after 5 blocks |

**Color generation** lives in the `Block` constructor:

```js
let r = Math.sin(0.3 * offset) * 55 + 200;
let g = Math.sin(0.3 * offset + 2) * 55 + 200;
let b = Math.sin(0.3 * offset + 4) * 55 + 200;
```

Change the phase offsets (`+2`, `+4`) to shift the palette, or the `55` amplitude to make colors more or less saturated.

---

## 🐛 Known Quirks

- **`WebGL context lost` alert** — If your GPU driver resets or the tab is suspended for a long time, the game shows an alert instead of recovering. Reload the page to continue.
- **Very fast initial speed ramp** — The `index * 0.005` acceleration is aggressive in longer runs. Lower the multiplier if you want a gentler curve.
- **No sound** — The game is silent by design; there's no audio layer.

---

## 📱 Browser Support

Works in any browser with WebGL and `requestAnimationFrame`:

- Chrome / Edge (desktop + Android)
- Safari (macOS + iOS 12+)
- Firefox (desktop + Android)

Touch events use `{ passive: false }` so `preventDefault()` can suppress scroll and zoom during play.

---

## 📄 License

This is a standalone educational/demo project. The Three.js and GSAP libraries retain their respective licenses (MIT for Three.js, and GSAP's standard "no charge for most uses" license — check [greensock.com/licensing](https://greensock.com/licensing/) for commercial terms).

You're free to modify and redistribute the game code as you see fit.

---

## 🙏 Credits

- Original concept: classic "Tower Bloxx" / "Stack" genre
- 3D engine: [Three.js](https://threejs.org/)
- Animation: [GSAP](https://greensock.com/gsap/)
