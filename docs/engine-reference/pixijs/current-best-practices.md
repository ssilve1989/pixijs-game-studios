# PixiJS v8 — Current Best Practices

Last verified: 2026-04-03

Practices that differ from v7 or from what the LLM may suggest based on older training data.

---

## Application Initialization

Always use async init. Never use the old synchronous constructor pattern.

```typescript
const app = new Application()
await app.init({
  width: 800,
  height: 600,
  backgroundColor: 0x1a1a2e,
  resolution: window.devicePixelRatio || 1,
  autoDensity: true,
})
document.body.appendChild(app.canvas) // not app.view
```

## Asset Loading

Use the `Assets` system for all resource loading. Never use `Texture.from(url)` without loading first.

```typescript
// Register upfront (manifest-based approach recommended for larger games)
await Assets.init({ manifest: 'assets/manifest.json' })

// Or load individually
const texture = await Assets.load('bunny.png')
const sprite = new Sprite(texture)

// Bundle loading
await Assets.loadBundle('level-1')
```

## TypeScript — Application Generic

```typescript
import { Application, Renderer } from 'pixi.js'

// Correct v8 generic signature
const app = new Application<Renderer<HTMLCanvasElement>>()
```

## Container Generic Typing (v8.1+)

Use typed containers to catch child type mismatches at compile time.

```typescript
import { Container, Sprite } from 'pixi.js'

// Only accepts Sprite children — type error if you add wrong type
const spriteLayer = new Container<Sprite>()
```

## Graphics — Draw Order

Shape first, then style. Never use `beginFill/endFill`.

```typescript
const g = new Graphics()

// Fill
g.rect(0, 0, 100, 50).fill(0xff0000)

// Stroke
g.circle(50, 50, 30).stroke({ width: 2, color: 0xffffff })

// Fill + stroke
g.roundRect(0, 0, 100, 50, 8).fill(0x333333).stroke({ width: 1, color: 0x666666 })

// Hole
g.rect(0, 0, 200, 200).fill(0xff0000)
g.rect(50, 50, 100, 100).cut() // cuts a hole
```

## Particles

Use `Particle` + `ParticleContainer`, not `Sprite` + `ParticleContainer`.

```typescript
import { ParticleContainer, Particle } from 'pixi.js'

const container = new ParticleContainer()
container.boundsArea = new Rectangle(0, 0, 800, 600) // required

const particle = new Particle({ texture })
particle.x = 100
particle.y = 100
container.addParticle(particle)
```

## Ticker / Game Loop

```typescript
app.ticker.add((ticker) => {
  const dt = ticker.deltaTime   // normalized (1 = 60fps frame)
  const ms = ticker.elapsedMS   // actual ms since last frame
  update(dt)
})
```

## BlurFilter (v8.17+)

If you need visual parity with pre-8.17 blur behavior:

```typescript
new BlurFilter({ blur: 8, legacy: true })
```

## Canvas Fallback (v8.16+)

For PWA deployments targeting low-end devices, enable automatic canvas fallback:

```typescript
await app.init({
  preference: 'webgl', // tries webgl, falls back to canvas automatically
})
```

## Shader Uniforms

Always declare explicit types in v8.

```typescript
const filter = new Filter({
  glProgram: GlProgram.from({
    vertex: vertSrc,
    fragment: fragSrc,
  }),
  resources: {
    myUniforms: {
      uTime: { value: 0, type: 'f32' },
      uColor: { value: new Float32Array([1, 0, 0, 1]), type: 'vec4<f32>' },
    }
  }
})
```

## PWA-Specific Recommendations

- Use `autoDensity: true` + `resolution: window.devicePixelRatio` for sharp rendering on HiDPI
- Preload all critical assets before rendering to avoid pop-in
- Use `Assets.loadBundle()` with manifest for cache-friendly asset grouping
- Pause the ticker when the page is hidden: `document.addEventListener('visibilitychange', ...)`
- Destroy the app cleanly on PWA lifecycle events to free GPU resources

## Texture UV Safety

Do not modify texture UVs after creation — it has no effect on sprites.
Create new textures instead; they are cheap to instantiate.
