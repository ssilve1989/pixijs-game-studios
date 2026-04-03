# PixiJS — Version Reference

| Field | Value |
|-------|-------|
| **Engine Version** | PixiJS v8.17.0 |
| **Release Date** | March 2026 |
| **Project Pinned** | 2026-04-03 |
| **Last Docs Verified** | 2026-04-03 |
| **LLM Knowledge Cutoff** | August 2025 |

## Knowledge Gap Warning

The LLM's training data likely covers PixiJS up to ~v8.5–8.8. Versions v8.9 through
v8.17 introduced changes the model may NOT know about. Always cross-reference this
directory before using PixiJS APIs.

## Risk Level: HIGH

PixiJS v8.17.0 is significantly beyond the LLM's training cutoff. Always verify
API suggestions against these reference docs.

## Post-Cutoff Version Highlights

| Version | Key Changes |
|---------|-------------|
| v8.9–8.11 | Various fixes and performance improvements |
| v8.12.0 | Stability and rendering fixes |
| v8.13.0 | Deprecation message controls (`suppressDeprecations`, color formatting options) |
| v8.16.0 | Canvas 2D fallback renderer (experimental), tagged text styling, CubeTexture, `parseSync` |
| v8.17.0 | BlurFilter halving strength scheme changed (breaking visual output — use `legacy: true` to restore) |

## Verified Sources

- Official docs: https://pixijs.com/8.x/
- v8 Migration Guide: https://pixijs.com/8.x/guides/migrations/v8
- GitHub Releases: https://github.com/pixijs/pixijs/releases
- npm: https://www.npmjs.com/package/pixi.js
