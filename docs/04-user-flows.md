# 04 — User Flows

Core interactions a user performs in the builder.

## Adding Elements

### From the Palette (Sidebar)
1. User opens Sections or Elements tab in sidebar
2. Drags a component card from the palette onto the canvas
3. Drop zones highlight valid targets
4. On drop, a new node is created with `defaultProps` and inserted into the tree

### From the Toolbar (Insert Menu)
1. User clicks "+" or "Insert" in toolbar
2. A menu shows available component types
3. User selects one
4. Node is inserted after the currently selected node (or at the end of the current container, or at root level)

## Moving Elements

### Drag and Drop
1. User hovers a node → drag handle appears (grip icon)
2. User grabs the grip and drags
3. Valid drop targets highlight (based on `containerAccepts` rules)
4. On drop, node is moved from old position to new position
5. If the move is invalid (wrong parent type), the drop is rejected

### Keyboard
1. Select a node
2. Alt + Arrow Up/Down to reorder within the same parent
3. No cross-parent keyboard moves (use drag for that)

## Selecting Elements

1. Click a node to select it
2. Blue outline appears, property panel shows its properties
3. Click canvas background to deselect
4. Click another node to switch selection
5. Selection persists across view switches

## Editing Content

### Inline Editing
1. Double-click a text element → enters edit mode
2. Text becomes editable (contentEditable or input)
3. Click outside or press Escape to confirm
4. Changes update the node's `props.content`

### Property Panel
1. Select a node → property panel shows fields from `propertySchema`
2. User modifies a field → node props update immediately
3. Changes are reflected in real-time on canvas

### Style Panel
1. Select a node → style panel shows available style controls
2. Controls are context-sensitive (sections get different options than text)
3. Changes apply immediately

## Deleting Elements

1. Select a node
2. Press Delete key, or click the red X button
3. Node and all children are removed from the tree
4. Selection is cleared

## Copy / Paste

1. Select a node
2. Ctrl+C to copy (stores a deep clone in clipboard)
3. Ctrl+V to paste (inserts as sibling of selected node, or as child of selected container)

## Undo / Redo

1. Every mutation (add, move, delete, edit) creates a history entry
2. Ctrl+Z to undo
3. Ctrl+Shift+Z or Ctrl+Y to redo
4. History is a linear stack (no branching)

## Switching Views

1. User clicks Wireframe / Visual / Preview in the toolbar
2. Canvas re-renders with the appropriate view renderer
3. Selected node is preserved if it still exists
4. Sidebar content adapts to the current view

## Breakpoint Switching

1. User clicks Desktop / Tablet / Mobile icons in toolbar
2. Canvas container resizes to the breakpoint width (with smooth transition)
3. All responsive styles adjust accordingly
4. The `activeBreakpoint` is stored so style edits target the right breakpoint

## Reusable Components

### Save as Component
1. Select a section (or subtree)
2. Click "Save as Component" in context menu or toolbar
3. Enter a name and choose a category
4. The subtree is saved as a `ReusableComponent`
5. It appears in the palette under the chosen category

### Use a Saved Component
1. Find it in the palette (under its category)
2. Drag onto canvas like any other component
3. A deep clone of the saved tree is inserted
4. Editing the instance does NOT affect the saved original

### Fork from Existing
1. Select an instance of a saved component
2. Click "Fork" in context menu
3. The instance becomes a standalone subtree (no link to original)
4. User can edit freely

## Layer Tree

1. Sidebar shows a collapsible tree of all nodes
2. Each node shows type icon + label
3. Clicking a node in the tree selects it on canvas
4. Dragging a node in the tree reorders it (same as canvas drag)
5. Eye icon toggles visibility (hidden nodes are semi-transparent on canvas)
6. Lock icon prevents selection/editing (useful for complex layouts)
