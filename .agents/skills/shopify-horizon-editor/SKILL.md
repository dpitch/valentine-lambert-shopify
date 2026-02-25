---
name: shopify-horizon-editor
description: >
  Expert-level editing of Shopify's Horizon theme (and compatible themes). Use when the user wants to
  customize, extend, fix, or create sections, blocks, snippets, templates, or settings in a Shopify
  Horizon theme. Triggers on: "Shopify theme", "Horizon theme", "Liquid section", "theme block",
  "Shopify section", "shopify template", "theme editor", "Shopify CSS", "Shopify JS", editing .liquid
  files inside sections/, blocks/, snippets/, layout/, or templates/ directories, editing
  settings_schema.json, or any Shopify storefront theme development work.
---

# Shopify Horizon Theme Editor

## Quick Orientation

Before editing, understand the codebase structure. Horizon uses ~487 files:

```
layout/           # theme.liquid (master HTML shell), password.liquid
templates/        # JSON page templates (index, product, collection, etc.)
sections/         # Section Liquid files + section group JSON
blocks/           # Theme blocks (reusable) — note _ prefix convention
snippets/         # Rendering helpers (no schema, invisible to merchants)
assets/           # CSS (base.css), JS modules, SVG icons
config/           # settings_schema.json, settings_data.json
locales/          # Translation JSON files (en.default.json, etc.)
```

## Core Architecture Patterns

### 1. Universal Section Wrapper

The cornerstone pattern: `sections/section.liquid` (and `sections/_blocks.liquid`) accepts any theme block:

```liquid
{% capture children %}
  {% content_for 'blocks' %}
{% endcapture %}
{% render 'section', section: section, children: children %}
```

Rendering delegates to `snippets/section.liquid` which outputs background, overlay, and `.layout-panel-flex` content wrapper.

### 2. Block Delegation Pattern

Every block in `blocks/` is a thin wrapper:
1. Captures `{% content_for 'blocks' %}` if it has children
2. Delegates rendering to a snippet via `{% render %}`
3. Defines `{% schema %}` with `"tag": null`

### 3. Underscore Prefix Convention

`_`-prefixed blocks (e.g., `_content.liquid`, `_media.liquid`) are private/internal — used within specific parent sections, not in the general block picker. Often `"static": true` in template JSON.

### 4. Group Block = Layout Primitive

`blocks/group.liquid` renders via `snippets/group.liquid`. Flex container accepting any theme block. Settings: direction (row/column), alignment, gap, width, height, color scheme, background media, border, overlay, link, padding.

`blocks/_content.liquid` is a simplified variant (column-only, no background media).

### 5. Color Scheme Cascade

Sections apply `.color-{scheme.id}` CSS class. Children inherit or override via `inherit_color_scheme`. CSS variables generated per scheme in `snippets/color-schemes.liquid`.

### 6. CSS Variable System

All custom properties on `:root` in `snippets/theme-styles-variables.liquid`:
- Page widths: `--narrow/normal/wide-page-width`
- Responsive section heights with `svh` units
- 4 font families + h1-h6 presets with `clamp()` fluid typography
- Z-index layers: `-2` to `20`
- Spacing scale: `3xs` to `6xl`

### 7. JavaScript Architecture

ES modules with importmap under `@theme/` namespace. Core modules preloaded; component scripts load with `fetchpriority="low"`. No frameworks — native DOM APIs only. Keep bundles < 16KB minified.

### 8. Inline Style Snippets

```liquid
{% render 'spacing-style', settings: settings %}   {# padding #}
{% render 'size-style', settings: settings %}       {# width/height #}
{% render 'border-override', settings: settings %}  {# borders #}
{% render 'layout-panel-style', settings: settings %} {# flex alignment/gap #}
```

## Reference Files

- **[references/conventions.md](references/conventions.md)** — Complete coding standards for blocks, sections, snippets, templates, schemas, CSS, HTML, JS, Liquid, locales, settings. Read when creating or editing any theme file.
- **[references/architecture.md](references/architecture.md)** — Deep dive into Horizon file structure, section/block hierarchy, template JSON, rendering pipeline. Read when understanding how components connect.
- **[references/settings-reference.md](references/settings-reference.md)** — All input setting types, schema format, `visible_if`, color schemes. Read when adding or modifying settings.
- **[references/accessibility.md](references/accessibility.md)** — WCAG 2.2 AA patterns for modals, carousels, forms, headings, focus. Read when building interactive components.

## Workflows

### Creating a new section

1. Create `sections/my-section.liquid`
2. Use `{% content_for 'blocks' %}` to render child blocks
3. Delegate to `snippets/section.liquid` or write custom HTML
4. Add `{% schema %}` with settings, blocks (`@theme`, `@app`), and presets
5. Add translation keys to `locales/en.default.schema.json`
6. Reference in a template JSON file

### Creating a new theme block

1. Create `blocks/my-block.liquid` (or `_my-block.liquid` for private)
2. Set `"tag": null` in schema
3. Include `{{ block.shopify_attributes }}` on root element
4. Create matching snippet if logic is complex
5. Use `{% stylesheet %}` for scoped CSS (BEM naming)
6. Add translation keys to locale files

### Modifying existing components

1. Read the file first — understand schema, settings, rendering
2. Check which snippets it delegates to
3. Check which template JSON files reference it
4. Preserve existing patterns
5. Update locale files if adding/changing labels
6. Test with `shopify theme check`

### Adding JavaScript

1. Create `assets/my-component.js` as ES module
2. Add to importmap in `snippets/scripts.liquid`
3. Import from `@theme/component` for web component base
4. Use `defer`/`async` on script tags, no frameworks
