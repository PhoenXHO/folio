# 03 — Plugin API

Folio is a framework. The host app tells it what components exist, how to render them, and where data comes from.

## What the Host App Provides

### Component Definitions

Every component type is registered with a definition object:

```ts
interface ComponentDefinition {
  type: string;                    // unique identifier, e.g. "hero-banner"
  label: string;                   // display name, e.g. "Hero Banner"
  icon: React.ComponentType;       // icon for sidebar/palette
  category: string;                // grouping in palette, e.g. "layout", "content", "commerce"
  acceptsChildren: boolean;        // can this node have children?
  allowedChildTypes?: string[];    // if acceptsChildren, which types? undefined = any
  allowedParentTypes?: string[];   // which parent types can contain this? undefined = any
  defaultProps: Record<string, unknown>;          // initial props when created
  defaultStyles?: ResponsiveStyles;               // initial styles when created
  defaultChildren?: ComponentChildren[];           // pre-built children (for templates)
  propertySchema: PropertyField[];  // what appears in the property panel
}

interface PropertyField {
  key: string;
  label: string;
  type: "text" | "textarea" | "select" | "toggle" | "number" | "color" | "image" | "url";
  options?: { label: string; value: string }[];  // for select type
  defaultValue?: unknown;
}
```

### Component Renderers

How each component looks in visual and preview modes:

```ts
interface ComponentRenderer {
  type: string;
  render: (props: {
    node: BuilderNode;
    styles: string;
    children: React.ReactNode;
    isEditing: boolean;
  }) => React.ReactNode;
}
```

The host app provides one renderer per component type. Folio wraps it with selection overlays, drag handles, etc. in visual mode. In preview mode, it renders the component directly.

### Data Sources

Components that need external data (e.g. product grids) declare their data needs:

```ts
interface DataSource {
  id: string;                     // e.g. "products", "categories"
  fetch: (params: Record<string, unknown>) => Promise<unknown[]>;
}
```

Components reference data sources in their props. The host app controls what data is available.

### Theme

The host app provides theme tokens (colors, fonts, spacing). Folio uses them for:
- Builder chrome (sidebar, toolbar, outlines)
- Component rendering (passed as CSS variables or a theme object)

```ts
interface ThemeConfig {
  colors?: Record<string, string>;
  fonts?: Record<string, string>;
  // host-defined — Folio doesn't enforce a schema
}
```

## What Folio Provides

- The three view renderers (wireframe, visual, preview)
- Drag-and-drop system
- Selection and hover management
- Undo/redo
- Property panel (auto-generated from `propertySchema`)
- Style panel
- Layer tree
- Breakpoint switching
- Keyboard shortcuts
- Copy/paste
- Component palette (sections + elements)

## Builder Configuration

The host app creates a configured builder instance:

```tsx
import { createBuilder } from "@phoenxho/folio";

const builder = createBuilder({
  components: [sectionDef, rowDef, columnDef, textDef, imageDef, heroDef, ...],
  renderers: [sectionRenderer, rowRenderer, columnRenderer, textRenderer, ...],
  dataSources: [productsSource, categoriesSource],
  theme: myTheme,
  // optional configuration
  views: {
    wireframe: true,    // enable/disable views
    visual: true,
    preview: true,
  },
  features: {
    undo: true,
    copyPaste: true,
    reusableComponents: true,
  },
});

// In your app:
<builder.Provider>
  <builder.Canvas />
  <builder.Sidebar />
</builder.Provider>
```

## Reusable Components (Pro Feature)

Users can save a section configuration as a reusable component:

```ts
interface ReusableComponent {
  id: string;
  name: string;
  category: string;
  icon?: React.ComponentType;
  tree: BuilderNode[];  // the saved subtree
  createdBy: string;    // user ID
  isBuiltIn: boolean;   // host-provided vs user-created
}
```

This is registered as a new component definition dynamically. It appears in the palette alongside built-in components.
