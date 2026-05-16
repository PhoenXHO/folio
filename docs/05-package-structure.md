# 05 — Package Structure

## npm Package

**Name:** `@phoenxho/folio` (scoped) or `folio-builder` (unscoped)

## Directory Layout

```
folio/
├── src/
│   ├── core/                    # Core engine (framework-agnostic logic)
│   │   ├── tree.ts              #   Tree operations (add, remove, move, find)
│   │   ├── validation.ts        #   containerAccepts, tree constraints
│   │   ├── history.ts           #   Undo/redo stack management
│   │   └── serialization.ts     #   Snapshot helpers
│   │
│   ├── store/                   # State management
│   │   ├── builder-store.ts     #   Main Zustand store
│   │   └── types.ts             #   State interfaces
│   │
│   ├── registry/                # Component registry
│   │   ├── registry.ts          #   Register, get, list definitions
│   │   └── types.ts             #   ComponentDefinition, PropertyField, etc.
│   │
│   ├── views/                   # Three view renderers
│   │   ├── wireframe/
│   │   │   ├── WireframeCanvas.tsx
│   │   │   └── WireframeNode.tsx
│   │   ├── visual/
│   │   │   ├── VisualCanvas.tsx
│   │   │   ├── VisualNode.tsx
│   │   │   ├── SelectionOverlay.tsx
│   │   │   └── DragHandle.tsx
│   │   └── preview/
│   │       └── PreviewCanvas.tsx
│   │
│   ├── dnd/                     # Drag-and-drop system
│   │   ├── DndProvider.tsx
│   │   ├── DropZone.tsx
│   │   ├── SortableNode.tsx
│   │   └── DragOverlay.tsx
│   │
│   ├── sidebar/                 # Sidebar panels
│   │   ├── Sidebar.tsx
│   │   ├── ComponentPalette.tsx
│   │   ├── LayerTree.tsx
│   │   ├── PropertyPanel.tsx
│   │   └── StylePanel.tsx
│   │
│   ├── canvas/                  # Canvas container + viewport
│   │   ├── Canvas.tsx
│   │   ├── Viewport.tsx
│   │   └── BreakpointControls.tsx
│   │
│   ├── toolbar/                 # Top toolbar
│   │   ├── Toolbar.tsx
│   │   ├── ViewSwitcher.tsx
│   │   └── ActionButtons.tsx
│   │
│   ├── plugin/                  # Plugin API
│   │   ├── create-builder.ts    #   createBuilder() factory
│   │   └── types.ts             #   Plugin configuration interfaces
│   │
│   └── index.ts                 # Public exports
│
├── docs/                        # Specification documents
├── examples/                    # Example integrations
│   └── basic/                   #   Minimal Next.js example
├── package.json
├── tsconfig.json
├── tsup.config.ts               # Bundler config
└── README.md
```

## Public Exports

```ts
// Everything the host app needs:
export { createBuilder } from "./plugin/create-builder";
export type { BuilderNode, ResponsiveStyles, Breakpoint, View } from "./core/types";
export type { ComponentDefinition, PropertyField, ComponentRenderer, DataSource, ThemeConfig } from "./registry/types";
export type { BuilderConfig } from "./plugin/types";
export type { ReusableComponent } from "./store/types";
```

## Dependencies

**Required peer dependencies (host app provides):**
- `react` >= 18
- `react-dom` >= 18

**Bundled dependencies:**
- `@dnd-kit/core` — drag-and-drop
- `@dnd-kit/sortable` — sortable lists
- `zustand` — state management
- `nanoid` — ID generation

**Dev dependencies:**
- `tsup` — bundler
- `typescript`
- `tailwindcss` — for builder chrome styling (output as CSS, not a peer dep)

## Build Output

```
dist/
├── index.js           # ESM
├── index.cjs          # CJS
├── index.d.ts         # Types
└── styles.css         # Builder chrome styles
```

The host app imports:
```ts
import { createBuilder } from "@phoenxho/folio";
import "@phoenxho/folio/styles.css";
```

## Versioning

Semantic versioning. Breaking changes to the plugin API are major versions. New component types or features are minor. Bug fixes are patch.
