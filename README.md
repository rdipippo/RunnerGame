# Dash Runner

An endless 3D runner (Subway-Surfers style) built with **Three.js** for rendering and
**Rapier** for physics/collision. Everything lives in a single `index.html` — no build step.

## Run it

ES modules + WASM need to be served over HTTP (not opened as a `file://`):

```bash
cd "RunnerGame"
python3 -m http.server 8080
# then open http://localhost:8080
```

(Any static server works — `npx serve`, etc.)

## Controls

| Action       | Keyboard        | Touch        |
|--------------|-----------------|--------------|
| Switch lane  | ← / → (or A/D)  | swipe L/R    |
| Jump         | ↑ / W / Space   | swipe up/tap |
| Slide        | ↓ / S           | swipe down   |

## How it works

- **Three.js** renders the scene: scrolling textured floor, glowing rails, recycled
  parallax buildings, a box-character with a run/slide animation, shadows and fog.
- **Rapier** drives the physics: the player is a *kinematic capsule* moved through a
  `KinematicCharacterController`, which performs collision-aware movement against the
  obstacle colliders each frame. A collision with an obstacle ends the run.
- The track is generated endlessly in **rows**: obstacles (jump-over barriers,
  slide-under gates, lane-blocking trains) and coins are spawned ahead and recycled
  behind the player. Difficulty scales with speed.

Obstacle vertical spans are tuned so each has a counter:
- **Orange barrier** — low, jump it.
- **Yellow gate** — high, slide under it.
- **Blue train** — full height, change lanes.
