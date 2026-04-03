# PixiJS v8 — Deprecated APIs

Last verified: 2026-04-03

"Don't use X → Use Y" quick-reference for agents.

---

## Class / Property Renames

| Don't Use (Old) | Use Instead (New) | Notes |
|----------------|-------------------|-------|
| `DisplayObject` | `Container` | Base class renamed |
| `NineSlicePlane` | `NineSliceSprite` | Renamed for clarity |
| `SimpleMesh` | `MeshSimple` | Renamed |
| `SimplePlane` | `MeshPlane` | Renamed |
| `SimpleRope` | `MeshRope` | Renamed |
| `Application.view` | `Application.canvas` | Property renamed |
| `container.name` | `container.label` | Property renamed |
| `container.cacheAsBitmap = true` | `container.cacheAsTexture(true)` | Method replaces property |
| `updateTransform()` | `onRender()` callback | Override hook renamed |

## Enum Constants → String Literals

Don't use numeric/enum constants — use string literals instead.

| Don't Use | Use Instead |
|-----------|-------------|
| `SCALE_MODES.NEAREST` | `'nearest'` |
| `SCALE_MODES.LINEAR` | `'linear'` |
| `WRAP_MODES.CLAMP` | `'clamp-to-edge'` |
| `WRAP_MODES.REPEAT` | `'repeat'` |
| `DRAW_MODES.TRIANGLES` | `'triangle-list'` |
| `DRAW_MODES.LINES` | `'line-list'` |

## Asset Loading

| Don't Use | Use Instead |
|-----------|-------------|
| `Texture.from(url)` (direct) | `await Assets.load(url)` first, then `Texture.from(url)` |
| `Assets.add('alias', 'src')` | `Assets.add({ alias: 'alias', src: 'src' })` |
| `Loader` (v7 class) | `Assets` system |

## Filters

| Don't Use | Use Instead |
|-----------|-------------|
| `@pixi/filter-*` packages | `pixi-filters` package (import as sub-modules) |

## Utility Namespace

| Don't Use | Use Instead |
|-----------|-------------|
| `import { utils } from 'pixi.js'; utils.isMobile.any()` | `import { isMobile } from 'pixi.js'; isMobile.any()` |
| `utils.hex2rgb()` etc. | Direct imports from `pixi.js` |

## Global Settings Object

| Don't Use | Use Instead |
|-----------|-------------|
| `settings.SCALE_MODE` | `AbstractRenderer.defaultOptions.scaleMode` |
| `settings.ADAPTER` | `DOMAdapter` |
| `settings.*` (any) | Per-renderer `defaultOptions` or constructor options |

## Asset Loader Parsers

| Don't Use | Use Instead |
|-----------|-------------|
| `loadParser` | `parser` |
| Old parser names (verbose) | Simplified parser names (see Assets docs) |

## Texture / BaseTexture

| Don't Use | Use Instead |
|-----------|-------------|
| `BaseTexture` | `TextureSource` variants (`ImageSource`, `CanvasSource`, etc.) |
| `BaseTexture.mipmap` | `autoGenerateMipmaps` |
| Modifying texture UVs after creation | Don't — create a new texture instead |
