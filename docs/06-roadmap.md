# 06 — Roadmap

Incremental versions with working software at each step.

---

## v0.1.0 — Foundation

Package scaffolding, data model, and core logic. No UI yet — just the engine, testable in isolation.

**Package setup**
- Initialize npm package with TypeScript + tsup
- Configure build output (ESM + CJS + types + CSS)
- Set up vitest for testing

**Data model**
- `BuilderNode`, `ResponsiveStyles` types
- Tree operations: `addNode`, `removeNode`, `moveNode`, `findNode`, `cloneTree`
- Validation: `containerAccepts(parentType, childType)` with constraint rules
- `nanoid` for stable node IDs

**Component registry**
- `registerComponent(def)`, `getComponentDef(type)`, `getAllComponents()`
- `ComponentDefinition` and `PropertyField` types

**State management**
- Zustand store: tree, selection, hover, breakpoint, view, clipboard, history
- History stack for undo/redo
- `getSnapshot()` / `loadSnapshot()` for serialization

**Deliverable:** Unit-tested core library. Can create a tree, add/remove/move nodes, validate constraints, serialize/deserialize. Published as internal alpha.

---

## v0.2.0 — Builder MVP

The minimum working builder. Visual view only, enough to add, select, move, and configure elements on a canvas.

**Canvas**
- Visual view renderer (`VisualNode`)
- `SelectionOverlay` (click to select, blue outline)
- `DragHandle` (grip icon, drag to move)
- Empty canvas state

**Drag & drop**
- `DndProvider` with @dnd-kit
- `SortableNode` wrapper
- `DropZone` between nodes (validated by `containerAccepts`)
- `DragOverlay` (preview follows cursor)
- Palette → canvas drops
- Canvas reordering

**Sidebar (basic)**
- Component palette (sections + elements tabs)
- Property panel (auto-generated from `propertySchema`)
- Inline text editing (double-click)

**Host integration**
- `createBuilder()` factory
- Accept component definitions and renderers
- Render the builder in a host app

**Deliverable:** Working builder in a test host app. Can drag components from palette, drop on canvas, select, edit properties, reorder via drag. No wireframe, no preview, no breakpoints.

---

## v0.3.0 — Complete Views

All three views, breakpoint switching, and full sidebar panels.

**Wireframe view**
- `WireframeNode` renderer — labeled dashed rectangles
- Sections → large boxes with labels
- Rows/columns → proportional strips
- Elements → small labeled placeholders
- Same DnD and selection as visual view

**Preview view**
- `PreviewNode` — renders host components with zero builder chrome
- No outlines, no drag handles, no drop zones
- Clickable links pass through

**View switching**
- Toolbar toggle: Wireframe / Visual / Preview
- Instant switch, same state, selected node preserved

**Breakpoints**
- Desktop (1280px), tablet (768px), mobile (375px)
- Smooth canvas transition between sizes
- Responsive style support in property/style panels

**Full sidebar**
- Layer tree (collapsible, click to select, drag to reorder)
- Style panel (context-sensitive controls per node type)
- Visibility toggle and lock icons in layer tree

**Deliverable:** Three views, breakpoints, full sidebar. The core building experience is complete.

---

## v0.4.0 — Plugin API

Production-ready host app integration. The package is now truly pluggable.

**Plugin system**
- `createBuilder(config)` with full configuration
- Component definitions with `propertySchema`
- Component renderers (visual mode)
- Data sources (for dynamic content like product grids)
- Theme configuration

**Host decoupling**
- Folio has zero commerce-specific code
- All business logic comes from the host via the plugin API
- Builder chrome styling is themeable

**Keyboard shortcuts**
- Delete, Ctrl+Z / Ctrl+Shift+Z (undo/redo)
- Ctrl+C / Ctrl+V (copy/paste)
- Alt+Arrow (reorder within parent)
- Escape (deselect / exit editing)

**Example integration**
- Minimal Next.js example app in `examples/basic/`
- Shows how to register components, renderers, and data sources
- Demonstrates all three views

**Deliverable:** Fully pluggable builder. Example app demonstrates integration. Package is ready for early adopters.

---

## v0.5.0 — Ship Ready

Polish, testing, and documentation. Ready for npm publish.

**Quality**
- Full test suite (unit + component tests)
- Accessibility audit (keyboard nav, ARIA labels, focus management)
- Performance (large trees, virtualized layer tree if needed)
- Error boundaries and graceful failure

**Documentation**
- API reference (auto-generated from TSDoc)
- Integration guide (getting started in 5 minutes)
- Component definition guide
- Custom renderers guide
- Data sources guide

**Package publishing**
- npm publish as `@phoenxho/folio`
- CI/CD pipeline (build + test + publish)
- Changelog generation

**Deliverable:** Published npm package with docs and example. Anyone can `npm install @phoenxho/folio` and build a page builder.

---

## v1.0.0 — Stable Release

Bug fixes from early adopters, final polish.

- Fix issues reported by early users
- Performance benchmarks and optimization
- Migration guide if any API changes from v0.x
- Official v1.0 announcement

---

## Future (Pro)

Features for `@phoenxho/folio-pro` (private repo, paid):

- Reusable components (save, fork, share)
- Collaborative editing (multi-user cursors)
- Export to HTML / CSS / React code
- CMS integrations (WordPress, Strapi, Contentful)
- Template marketplace
- Advanced version history
- Custom theme editor
- Analytics and usage tracking
