# Technical Preferences

<!-- Populated by /setup-engine. Updated as the user makes decisions throughout development. -->
<!-- All agents reference this file for project-specific standards and conventions. -->

## Engine & Language

- **Engine**: PixiJS v8.17.0
- **Language**: TypeScript
- **Rendering**: WebGL2 (primary), WebGPU (opt-in experimental), Canvas 2D (fallback via `preference: 'canvas'`)
- **Physics**: None built-in — add Matter.js or Planck.js if needed

## Naming Conventions

- **Classes**: PascalCase (e.g., `PlayerEntity`)
- **Variables/Functions**: camelCase (e.g., `moveSpeed`, `takeDamage()`)
- **Events**: camelCase past tense (e.g., `onHealthChanged`)
- **Files**: PascalCase for classes, camelCase for modules (e.g., `PlayerEntity.ts`, `inputManager.ts`)
- **Scenes/Prefabs**: N/A — PixiJS uses Container hierarchies, not scene files
- **Constants**: UPPER_SNAKE_CASE (e.g., `MAX_HEALTH`)
- **Types/Interfaces**: PascalCase; prefix interfaces with `I` (e.g., `IEntity`)

## Performance Budgets

- **Target Framerate**: 60fps [TO BE CONFIGURED]
- **Frame Budget**: 16.6ms [TO BE CONFIGURED]
- **Draw Calls**: [TO BE CONFIGURED — typical web budget: <100]
- **Memory Ceiling**: [TO BE CONFIGURED]

## Testing

- **Framework**: Vitest (Vite-native, TypeScript-first)
- **Minimum Coverage**: [TO BE CONFIGURED]
- **Required Tests**: Balance formulas, gameplay systems

## Forbidden Patterns

<!-- Add patterns that should never appear in this project's codebase -->
- [None configured yet — add as architectural decisions are made]

## Allowed Libraries / Addons

<!-- Add approved third-party dependencies here -->
- [None configured yet — add as dependencies are approved]

## Architecture Decisions Log

<!-- Quick reference linking to full ADRs in docs/architecture/ -->
- [No ADRs yet — use /architecture-decision to create one]
