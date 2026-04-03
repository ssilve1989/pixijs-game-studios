# PixiJS v8 — Breaking Changes

Last verified: 2026-04-03

This document covers breaking changes from v7→v8 and notable changes within v8.x
that agents must be aware of.

---

## v7 → v8 Breaking Changes

### Package Structure
Single package replaces sub-packages.

```typescript
// OLD (v7)
import { Application } from '@pixi/app'
import { Sprite } from '@pixi/sprite'

// NEW (v8)
import { Application, Sprite } from 'pixi.js'
```

### Async Initialization
Application initialization is now async.

```typescript
// OLD (v7)
const app = new Application({ width: 800, height: 600 })

// NEW (v8)
const app = new Application()
await app.init({ width: 800, height: 600 })
```

### Texture Architecture
`BaseTexture` is removed. Use `TextureSource` variants instead.

| Old | New |
|-----|-----|
| `new BaseTexture(image)` | `new ImageSource({ resource: image })` |
| `BaseTexture.from(url)` | Load via `Assets.load()` first |

Available TextureSources: `ImageSource`, `CanvasSource`, `VideoSource`, `BufferSource`, `CompressedSource`

Textures no longer manage resource loading — resources must be loaded upfront via `Assets`.

### Graphics API Overhaul
Method names shortened and draw order reversed (shape first, then style).

```typescript
// OLD (v7)
graphics.beginFill(0xff0000)
graphics.drawRect(0, 0, 100, 100)
graphics.endFill()

// NEW (v8)
graphics.rect(0, 0, 100, 100).fill(0xff0000)
```

| Old Method | New Method |
|-----------|-----------|
| `drawRect()` | `rect()` |
| `drawCircle()` | `circle()` |
| `drawEllipse()` | `ellipse()` |
| `drawRoundedRect()` | `roundRect()` |
| `drawPolygon()` | `poly()` |
| `drawStar()` | `star()` |
| `beginHole()` / `endHole()` | `.cut()` |
| `lineTo()` / `moveTo()` | unchanged |
| `GraphicsGeometry` | `GraphicsContext` |

### Container-Only Children
Only `Container` can have children. `Sprite`, `Mesh`, `Graphics` etc. cannot.

```typescript
// OLD (v7) — worked
const sprite = new Sprite(texture)
sprite.addChild(someChild)

// NEW (v8) — must wrap in Container
const container = new Container()
const sprite = new Sprite(texture)
container.addChild(sprite)
container.addChild(someChild)
```

### ParticleContainer Redesign
No longer accepts `Sprite` children. Requires `Particle` class implementing `IParticle`.

```typescript
// OLD (v7)
const pc = new ParticleContainer()
pc.addChild(new Sprite(texture))

// NEW (v8)
const pc = new ParticleContainer()
const particle = new Particle(texture)
pc.addParticle(particle)
// Access via pc.particleChildren (not pc.children)
// Must set pc.boundsArea manually
```

### Shader System
Textures are no longer uniforms; use the "resources" concept.

```typescript
// NEW (v8) — uniform type must be explicit
uniforms: {
  uTime: { value: 0, type: 'f32' },
  uResolution: { value: [800, 600], type: 'vec2<f32>' }
}
// Shader.from() now accepts { gl: '...', gpu: '...' } for WebGL/WebGPU
```

### Ticker Callback Signature
```typescript
// OLD (v7)
app.ticker.add((dt) => { /* dt is delta */ })

// NEW (v8)
app.ticker.add((ticker) => { const dt = ticker.deltaTime })
```

### constructor Signatures
Most constructors now accept a single options object.

```typescript
// OLD (v7)
new BlurFilter(8, 4, 1, 5)

// NEW (v8)
new BlurFilter({ blur: 8, quality: 4, resolution: 1, kernelSize: 5 })
```

### Assets.add() Signature
```typescript
// OLD (v7)
Assets.add('bunny', 'bunny.png')

// NEW (v8)
Assets.add({ alias: 'bunny', src: 'bunny.png' })
```

### getBounds() Return Type
```typescript
// OLD (v7) — returned Rectangle
const bounds = container.getBounds() // Rectangle
bounds.x, bounds.y, bounds.width, bounds.height

// NEW (v8) — returns Bounds object
const bounds = container.getBounds()
const rect = bounds.rectangle // access the Rectangle
```

### TypeScript Generic Change
```typescript
// OLD (v7)
const app = new Application<HTMLCanvasElement>()

// NEW (v8)
const app = new Application<Renderer<HTMLCanvasElement>>()
// or for WebGPU:
const app = new Application<WebGPURenderer<HTMLCanvasElement>>()
```

---

## Notable v8.x Changes

### v8.13.0 — Deprecation Warning Controls
```typescript
// Suppress all deprecation warnings
import { deprecation } from 'pixi.js'
deprecation.suppressDeprecations = true
```

### v8.16.0 — Canvas 2D Fallback Renderer
New optional fallback for environments without WebGL/WebGPU:
```typescript
await app.init({
  preference: 'canvas' // force canvas renderer
})
// Auto-fallback also available
```

### v8.17.0 — BlurFilter Behavior Change (BREAKING VISUAL)
BlurFilter now uses an optimized halving strength scheme by default.
**Visual output will differ from previous versions.**
```typescript
// Restore old behavior:
new BlurFilter({ blur: 8, legacy: true })
```
