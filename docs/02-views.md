# 02 — Views

Three parallel views over the same node tree. Users switch freely — no data loss, no "promotion" step. A hero section is always a hero section; it just renders differently.

## View 1: Wireframe

The planning view. Shows structure without real content.

**What it renders:**
- Every node becomes a dashed rectangle with a label
- Sections → large rectangles with "Section: Hero" or custom label
- Rows → horizontal strips
- Columns → proportional boxes within rows (respecting span)
- Elements → small labeled boxes ("Text", "Button", "Image")

**What it shows:**
- Layout structure at a glance
- Column proportions (span widths visible)
- Nesting hierarchy
- Drop zones for adding new nodes

**What it hides:**
- Actual text content (just shows "Text" label)
- Images (just shows "Image" placeholder)
- Colors, fonts, styling details
- Real component rendering

**Use case:** Plan your page layout before worrying about content. Think "wireframe tool."

## View 2: Visual

The building view. Shows real content with builder controls.

**What it renders:**
- Actual component renderers (the real text, images, buttons)
- Host-provided components render with their full visual output
- All styling is applied

**What it shows:**
- Real content (editable inline by double-click)
- Selection outline (blue border when selected)
- Hover outline (dashed border on hover)
- Drag handles (grip icon, only on hover or selection)
- Delete button (when selected)
- Element label badges (type name, on hover)
- Debug boundaries (optional, toggleable — shows element boundaries even when not hovered)

**What it hides:**
- Nothing — this is the full builder experience

**Use case:** Fill in content, adjust styling, configure components. Think "Figma edit mode."

## View 3: Preview

The clean view. Zero builder chrome.

**What it renders:**
- Same visual output as View 2 (real components, real styling)
- Responsive preview at selected breakpoint

**What it shows:**
- Exactly what the end user would see
- Clickable links (not intercepted by builder)
- Responsive sizing (desktop, tablet, mobile frames)

**What it hides:**
- All selection/hover outlines
- Drag handles
- Delete buttons
- Label badges
- Drop zones
- Sidebar panels

**Use case:** Review the final result. Think "Figma presentation mode."

## Switching Views

Views are switched via a toolbar toggle. The switch is instant — same state, different renderer. The selected node persists across view switches (if the node still exists).

## Implementation

Each view is a React component that receives the node tree and renders it differently:

```
<Builder>
  <ViewSwitcher />      // toolbar: wireframe | visual | preview
  <Sidebar />           // context-sensitive panel
  <Canvas>
    <NodeTreeRenderer view={activeView}>
      {tree.map(node => <NodeRenderer node={node} view={activeView} />)}
    </NodeTreeRenderer>
  </Canvas>
</Builder>
```

Each `NodeRenderer` delegates to the active view's renderer:
- Wireframe → `WireframeNodeRenderer`
- Visual → `VisualNodeRenderer` (wraps host component with selection overlay)
- Preview → `PreviewNodeRenderer` (renders host component with no chrome)
