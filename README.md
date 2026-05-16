# Folio

A visual page builder for React. Three parallel views — wireframe, visual, preview — over a single data model. Plugin-based architecture lets host apps define components, data sources, and styling.

## Open Core Model

**Folio** (this repo) is the free, open-source core. It includes everything needed to build a page builder: three views, drag-and-drop, component registry, styling, undo/redo, and more.

**Folio Pro** (private repo, paid) extends the core with advanced features: reusable components, collaboration, export to code, CMS integrations, and a template marketplace. Hosted as a separate npm package (`@phoenxho/folio-pro`) that plugs into the same API.

### What's Free

- Three views (wireframe, visual, preview)
- Component registry and palette
- Drag-and-drop with validation
- Selection, hover, inline editing
- Auto-generated property and style panels
- Layer tree
- Breakpoint switching (desktop, tablet, mobile)
- Undo/redo, copy/paste
- Keyboard shortcuts
- Plugin API for host app integration

### What's Pro

- Reusable components (save, fork, share)
- Collaborative editing (multi-user cursors)
- Export to clean HTML/CSS/React code
- CMS integrations (WordPress, Strapi, etc.)
- Template marketplace
- Version history (beyond simple undo)
- Custom theme editor
- Analytics and usage tracking

## Status

Pre-release. Specifications in progress.

## Docs

- [01 — Data Model](docs/01-data-model.md)
- [02 — Views](docs/02-views.md)
- [03 — Plugin API](docs/03-plugin-api.md)
- [04 — User Flows](docs/04-user-flows.md)
- [05 — Package Structure](docs/05-package-structure.md)
- [06 — Roadmap](docs/06-roadmap.md)
