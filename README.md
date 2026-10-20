# Brownian Graphe

**A seed-based generative system for bouncing graph networks.**

A catalogue of computational textile compositions for fashion, textile and surface design — algorithmically drawn, seed-documented, and ready for production.

---

## Overview

Brownian Graphe is a generative design system rather than a single artwork. Each composition is built from a small set of points — some connected, some free — bouncing inside the frame with simulated Brownian motion. Where points collide, they swap velocities and invert colour. Where they approach, the lines between them shift in brightness by distance.

The system is designed for:

- **Fashion houses** adapting network ornament for apparel and accessories
- **Textile studios** developing repeat patterns and yardage
- **Surface designers** working across print, wallpaper, and interior applications

Every composition can be licensed, adapted, or commissioned to a brief.

---

## Concept

A graph, when it is *set in motion*, becomes a swarm — links stretching, points colliding, structure emerging from pressure.

The network — points and their links — has always carried motion. Roads, constellations, weaving drafts, trade routes. A graph is not a picture of stillness; it is a picture of relationship under tension. Brownian Graphe translates that tension into code. Each composition begins with a small set of points and a graph matrix, and unfolds through collision, drift, and rebound until the frame fills with a self-organising swarm.

The palette, the number of points, the density of the graph, the boundary buffer, and the ball diameter are all derived from a single numeric seed.

Unlike the earlier volumes in the series, **Brownian Graphe is a live, interactive system.** The plate breathes. You may click anywhere on it to add a new point, and the graph will rewire itself around it. But the archive, surfaces, and framed plate remain static — they are the print-ready compositions.

---

## Features

- **Seed-based generation** — every composition is defined by a numeric seed and can be regenerated exactly
- **Deterministic output** — the same seed always produces the same composition at any given frame
- **Animated plate** — the live Plate 001 animates continuously via Brownian motion
- **Static catalogue** — the framed plate, surfaces, and archive thumbnails are all frozen single frames, suitable for print
- **Interactive** — click anywhere on the live plate to add a new point and rewire the graph
- **Symmetric graph matrix** — every point is linked to a random subset of the others, mirrored across the diagonal
- **Distance-based brightness** — link colour fades with distance, from white to black
- **Collision physics** — points swap velocities and invert colour on contact
- **Adaptive surfaces** — one seed applied across print, scarf, textile, and wall formats
- **Archive** — eight curated seeds available for immediate loading
- **Download** — export the current frame as a high-resolution PNG
- **Keyboard shortcuts** — `R` for new seed, `S` to save

---

## Project Structure

```
.
├── index.html          # Main catalogue page
├── images/
│   ├── fav.svg         # Favicon
│   ├── tote.png        # Mockup: tote bag
│   ├── tee.png         # Mockup: t-shirt
│   └── cushion.png     # Mockup: cushion
└── README.md
```

---

## How It Works

### The Seed

A numeric seed (a large integer) initializes a deterministic pseudo-random generator. From this seed, the system derives:

- Two background colours for the diagonal gradient (from a palette of 71 tones)
- Number of starting points (typically 3–15)
- Minimum links per point
- Boundary buffer (the margin points bounce inside)
- Ball diameter
- Initial velocity for each point

Because the generator is deterministic, the same seed always produces the same composition — on any device, at any time.

### The Graph

Each composition begins with a symmetric adjacency matrix. For each point, the system picks a random subset of the others to link to — and mirrors those choices across the diagonal so the graph is undirected.

When you click on the live plate to add a point:

1. A new row and column are appended to the matrix.
2. The system picks a random number of existing points to connect the new point to.
3. A new velocity is generated.
4. The graph redraws itself around the new node.

### The Motion

Each point carries a velocity vector. On every animation frame:

- The position is updated by `velocity × step`, where step is a fraction of the ball diameter.
- If the point crosses the boundary buffer, its velocity component is reflected.
- A small random perturbation is added to the velocity, then the magnitude is clamped.
- If two points come within one ball diameter, they swap velocity vectors and invert colour.

This produces a system that never fully settles — always moving, always reorganising, always on the edge of collision.

### The Links

Each link is drawn as a straight line between two points. Its colour is derived from the distance between the two points:

- Close points → bright lines (near white)
- Far points → dark lines (near black)

So the graph reads as a shifting web, denser where points cluster and dimmer where they spread.

### The Surfaces

The same seed is rendered across four surface formats. These are **static, frozen at frame zero** — they represent the print-ready composition, not the animation.

| Surface  | Aspect | Material          |
|----------|--------|-------------------|
| Print    | 1 : 1  | Cotton rag        |
| Scarf    | 3 : 1  | Twill silk        |
| Textile  | 4 : 3  | Fabric yardage    |
| Wall     | 2 : 3  | Wallpaper         |

Each surface uses the same underlying seed and structural logic — only the repeat, orientation, and scale change.

### Animation

Brownian Graphe is animated on the live plate. The animation behaves like this:

- **Plate 001** — animated continuously via `requestAnimationFrame`. Every frame advances the motion by one step.
- **Framed plate, surfaces, archive thumbnails, cover** — rendered as single, static frames at frame zero. They do not animate.
- **Download** — pauses the plate animation, captures the current frame as a PNG, then resumes.
- **Tab visibility** — animation pauses when the tab is hidden and restarts cleanly when you return.
- **Reduced motion** — if the user has `prefers-reduced-motion: reduce`, the plate renders a single frame and does not animate.

The reason only the plate animates: the catalogue exists to present print-ready compositions. Surfaces and archive thumbnails must be comparable at a glance, and eight simultaneously animating thumbnails would be heavy and visually confusing. The live plate, however, is where the character of Brownian Graphe — motion, collision, tension — is expressed.

---

## Usage

### In the browser

1. Open `index.html` in any modern browser.
2. The Plate 001 composition animates on its own.
3. **Click or tap anywhere on the live plate** to add a new point.
4. Click **New Seed** to generate a new composition.
5. Click **Download** to save the current frame as a PNG.
6. Scroll to the **Archive** section and click any plate to load it into Plate 001.

### Keyboard shortcuts

| Key | Action          |
|-----|-----------------|
| `R` | New seed        |
| `S` | Save as PNG     |

### Reproducing a composition

Each composition is identified by an 8-digit seed label displayed in the metadata panel. To reproduce a specific composition statically, note the seed and regenerate it programmatically:

```js
const rng = new RandomGenerator(seed);
const features = buildFeatures(rng, canvasSize);
renderComposition(canvas, features, rng, false);
```

For an animated reproduction, use the same pattern with a `requestAnimationFrame` loop and call `renderComposition(canvas, features, rng, true)` each frame.

---

## Technical Notes

- **No build step.** The system is a single HTML file with inline CSS and JavaScript.
- **No dependencies.** All drawing is done with the native Canvas 2D API.
- **Deterministic.** The `RandomGenerator` class uses a xorshift-based PRNG seeded by an integer, so identical seeds produce identical outputs.
- **Animation isolated to the plate.** Only the live Plate 001 uses `requestAnimationFrame`. All other canvases render a single frame and stop.
- **Feature isolation.** Cover, framed plate, surfaces, and archive thumbnails each derive their own feature set from their own local RNG, without disturbing the main plate's state.
- **Interactive.** Click / tap handlers are attached to the canvas itself, not the container, so only real canvas interactions add points.
- **Responsive.** The layout adapts from large desktop down to very small mobile devices (tested at 360px viewport width).
- **Accessible.** Supports `prefers-reduced-motion`. Pinch-zoom is enabled.

### Browser support

Tested in current versions of:

- Chrome / Edge
- Firefox
- Safari (desktop and iOS)

---

## Licensing

All Brownian Graphe compositions are **seed-documented** and available for licensing across textile, surface, and print applications.

- **Standard licenses** cover single-product production runs.
- **Commercial use, custom editions, or exclusive rights** are available on request.

Each license is issued against a specific seed ID. Regeneration of the same seed produces the identical composition — ensuring reproducibility between artist, studio, and manufacturer.

For licensing enquiries: [reyhanehdaneshdoost@gmail.com](mailto:reyhanehdaneshdoost@gmail.com)

---

## Commission

Brownian Graphe is a generative design system, not a fixed artwork. It can be adapted for specific briefs:

| Service     | Description                                                       |
|-------------|-------------------------------------------------------------------|
| Licensing   | Existing seeds from the archive, licensed for production use      |
| Commission  | New compositions designed to your palette, repeat, and product    |
| Systems     | A private generative tool built for your studio's ongoing use     |

To begin a conversation: [reyhanehdaneshdoost@gmail.com](mailto:reyhanehdaneshdoost@gmail.com)

---

## Series

Brownian Graphe is part of a computational textile series. Each volume approaches ornament from a different structural angle:

| Volume             | Structure              | Motion                     |
|--------------------|------------------------|----------------------------|
| Girih 1            | Islamic geometric      | Static                     |
| Arachne            | Rotating rings         | Static                     |
| Baroque Me Baby    | Baroque frames         | Static                     |
| Bezier 1           | Concentric curves      | Static                     |
| Bezier 2           | Single rotating curve  | Animated (plate)           |
| **Brownian Graphe**| Graph networks         | **Animated + interactive** |

The series is designed as a coherent whole — same page structure, same seed logic, same licensing and commission terms — so that each volume can be presented individually or as part of a larger body of work.

---

## Credits

- **Design & Generative System** — Reyhaneh Daneshdoost
- **Typefaces** — Cormorant Garamond · DM Mono
- **Platform** — Reyrove Studio
- **Edition** — Brownian Graphe, Autumn 2026

### On AI tools

Where technical obstacles were encountered, AI tools were used for debugging and code optimization. Every structural, aesthetic, and conceptual decision remained the artist's own.

---

## Links

- Website — [reyrove.github.io](https://reyrove.github.io/)
- Instagram — [@rey._.rove](https://www.instagram.com/rey._.rove/)
- LinkedIn — [Reyhaneh Daneshdoost](https://www.linkedin.com/in/reyhaneh-daneshdoost-730481160/)
- X — [@reyrove](https://x.com/reyrove)

---

© Brownian Graphe · All compositions reproducible by seed · Computational Textile Design