# 01 — Data Model

The node tree is the single source of truth. All three views (wireframe, visual, preview) read from the same tree and render it differently.

## Node Tree

Every page is a tree of `BuilderNode` objects.

```ts
interface BuilderNode {
  id: string;           // unique, e.g. nanoid
  type: string;         // component type: "section", "row", "column", "text", "image", etc.
  props: Record<string, unknown>;    // component-specific props (text content, image URL, etc.)
  styles: ResponsiveStyles;          // per-breakpoint tailwind classes or CSS
  children: BuilderNode[];           // child nodes (empty array for leaf elements)
  parentId: string | null;           // parent node ID (null for root-level)
}
```

### ResponsiveStyles

```ts
interface ResponsiveStyles {
  base: string;      // default (mobile-first)
  sm?: string;       // ≥640px
  md?: string;       // ≥768px
  lg?: string;       // ≥1024px
  xl?: string;       // ≥1280px
}
```

## Tree Constraints

The tree is not free-form. Parent-child relationships are constrained:

| Parent    | Allowed Children                             |
|-----------|----------------------------------------------|
| Page root | `section`                                    |
| section   | `row`                                        |
| row       | `column`                                     |
| column    | any leaf element (text, image, button, etc.) |
| leaf      | none (empty children array)                  |

Host apps can register additional container types that accept specific children. The `containerAccepts(parentType, childType)` function validates all drop/insert operations.

## Selection State

```ts
interface BuilderState {
  tree: BuilderNode[];
  selectedNodeId: string | null;
  hoveredNodeId: string | null;
  activeBreakpoint: Breakpoint;
  activeView: View;               // "wireframe" | "visual" | "preview"
  clipboard: BuilderNode | null;  // for copy/paste
  history: BuilderNode[][];       // for undo
  historyIndex: number;
}
```

## Node ID References

Node IDs are stable across the lifetime of a session. They are generated on creation and never change. This makes selection, drag-and-drop, and undo/redo straightforward — operate on IDs, not indices.

## Serialization

The tree serializes to plain JSON. No class instances, no functions, no React elements. This means:
- Easy to persist (database, localStorage)
- Easy to transmit (API payload)
- Easy to diff (two trees are comparable)

The host app is responsible for persistence. Folio provides `getSnapshot()` and `loadSnapshot()` helpers.
