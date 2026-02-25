# Horizon Theme Coding Conventions

Complete coding standards extracted from Shopify's official Horizon theme repository.

## Table of Contents

1. [Blocks](#blocks)
2. [Sections](#sections)
3. [Snippets](#snippets)
4. [Templates](#templates)
5. [Schemas](#schemas)
6. [CSS Standards](#css-standards)
7. [HTML Standards](#html-standards)
8. [JavaScript Standards](#javascript-standards)
9. [Liquid Standards](#liquid-standards)
10. [Locales](#locales)
11. [Theme Settings](#theme-settings)

---

## Blocks

### Basic Block Structure

```liquid
{% doc %}
  Block description and usage examples

  @example
  {% content_for 'block', type: 'block-name', id: 'unique-id' %}
{% enddoc %}

<div
  {{ block.shopify_attributes }}
  class='block-name'
>
  <!-- Block content using block.settings -->
</div>

{% stylesheet %}
  /* Scoped CSS for this block — BEM structure
     Only for components exclusively in this block.
     Shared CSS goes in assets/base.css */
{% endstylesheet %}

{% schema %}
{
  "name": "Block Name",
  "settings": [],
  "presets": []
}
{% endschema %}
```

### Static Block Usage

Static blocks are defined inside templates and cannot be repositioned by merchants:

```json
{
  "type": "block-name",
  "static": true,
  "id": "unique-static-id"
}
```

### Nested Blocks

Blocks can accept child blocks. Use `{% content_for 'blocks' %}` to render children:

```liquid
{% capture children %}
  {% content_for 'blocks' %}
{% endcapture %}

<div {{ block.shopify_attributes }}>
  {{ children }}
</div>

{% schema %}
{
  "name": "Container Block",
  "tag": null,
  "blocks": [
    { "type": "@theme" },
    { "type": "@app" }
  ]
}
{% endschema %}
```

### Key Rules

- Always set `"tag": null` — the theme controls its own HTML output
- Always include `{{ block.shopify_attributes }}` on the root element
- Use `{% stylesheet %}` for scoped CSS, not inline `<style>` tags
- Prefer delegating to snippets for complex rendering
- Private blocks use `_` prefix (e.g., `_my-internal-block.liquid`)
- Blocks with presets appear in the theme editor block picker
- Static blocks appear in template JSON with `"static": true`

---

## Sections

### Basic Section Structure

```liquid
{% doc %}
  Section description

  @example
  In a template JSON:
  { "type": "my-section", "blocks": {} }
{% enddoc %}

{% capture children %}
  {% content_for 'blocks' %}
{% endcapture %}

{% render 'section', section: section, children: children %}

{% schema %}
{
  "name": "t:names.my_section",
  "tag": "section",
  "blocks": [
    { "type": "@theme" },
    { "type": "@app" }
  ],
  "settings": [],
  "presets": [
    {
      "name": "t:names.my_section",
      "category": "t:categories.content"
    }
  ]
}
{% endschema %}
```

### Using `snippets/section.liquid`

Most sections delegate to `snippets/section.liquid` which provides:
- `.section-background` for full-bleed color scheme backgrounds
- `.section` div with width class (`section--page-width` or `section--full-width`)
- Background media (image/video)
- Overlay layer
- `.layout-panel-flex` content wrapper with flex direction

### Section Groups

Header and footer are rendered as section groups via `{% sections 'header-group' %}` and `{% sections 'footer-group' %}`.

Sections restrict group usage with:
```json
"enabled_on": { "groups": ["header"] }
"disabled_on": { "groups": ["footer", "aside"] }
```

### Key Rules

- Sections need presets to appear in theme editor's section picker
- Use `{% content_for 'blocks' %}` for dynamic blocks
- Use `{% content_for 'block', type: 'type', id: 'id' %}` for static blocks
- Max 25 sections per template, 50 blocks per section
- Section settings accessed via `section.settings.setting_id`

---

## Snippets

### Basic Snippet Structure

```liquid
{% doc %}
  Description of what this snippet renders

  @param {String} title - The title to display
  @param {Boolean} [show_icon] - Whether to show an icon

  @example
  {% render 'my-snippet', title: 'Hello', show_icon: true %}
{% enddoc %}

<div class="my-snippet">
  <h2>{{ title }}</h2>
  {% if show_icon %}
    {% render 'icon', icon: 'checkmark' %}
  {% endif %}
</div>
```

### Key Rules

- Snippets are invisible to merchants in the theme editor
- Cannot access external variables — only receive data through explicit parameters
- Global objects (product, collection, section) remain accessible
- Use `{% render %}` tag (not `{% include %}` which is deprecated)
- Use LiquidDoc (`{% doc %}`) annotations with `@param` and `@example`
- Name with kebab-case (e.g., `product-card`, `cart-summary`)
- No `{% schema %}` tag — snippets don't have settings

---

## Templates

### JSON Template Structure

```json
{
  "layout": "theme",
  "sections": {
    "section-id": {
      "type": "section-type",
      "blocks": {
        "block-id": {
          "type": "block-type",
          "settings": { ... }
        }
      },
      "block_order": ["block-id"],
      "settings": { ... }
    }
  },
  "order": ["section-id"]
}
```

### Available Template Types

| Template | Purpose |
|----------|---------|
| `index.json` | Home page |
| `product.json` | Product pages |
| `collection.json` | Collection pages |
| `blog.json` | Blog listing |
| `article.json` | Blog post |
| `page.json` | Static pages |
| `cart.json` | Cart page |
| `search.json` | Search results |
| `list-collections.json` | Collection directory |
| `404.json` | Not found page |
| `password.json` | Password page |
| `gift_card.liquid` | Gift card (Liquid only) |

### Alternate Templates

Create alternate templates with dot notation: `page.contact.json`, `product.featured.json`.

### Static Blocks in Templates

```json
{
  "block-id": {
    "type": "_my-private-block",
    "static": true,
    "settings": { ... }
  }
}
```

### Key Rules

- Max 1000 JSON templates across all types
- Use JSON templates (not Liquid) for section support
- Blocks can nest up to 8 levels deep in Horizon
- Use `{{ closest.product }}` to access nearest ancestor's product context
- `block_order` array determines rendering order

---

## Schemas

### Standard Schema Properties

```json
{
  "name": "t:names.section_name",
  "tag": "section",
  "class": "optional-css-class",
  "limit": 1,
  "settings": [],
  "blocks": [],
  "max_blocks": 16,
  "presets": [],
  "locales": {},
  "enabled_on": {},
  "disabled_on": {}
}
```

### Common Settings Groups in Horizon

**Layout settings:**
```json
{ "type": "select", "id": "content_direction", "options": [
  { "value": "column", "label": "t:options.column" },
  { "value": "row", "label": "t:options.row" }
]},
{ "type": "select", "id": "horizontal_alignment", "options": [
  { "value": "start", "label": "t:options.start" },
  { "value": "center", "label": "t:options.center" },
  { "value": "end", "label": "t:options.end" }
]},
{ "type": "select", "id": "gap", "options": [
  { "value": "none", "label": "t:options.none" },
  { "value": "small", "label": "t:options.small" },
  { "value": "medium", "label": "t:options.medium" },
  { "value": "large", "label": "t:options.large" }
]}
```

**Size settings:**
```json
{ "type": "select", "id": "section_width", "options": [
  { "value": "page-width", "label": "t:options.page_width" },
  { "value": "full-width", "label": "t:options.full_width" }
]},
{ "type": "select", "id": "section_height", "options": [
  { "value": "auto", "label": "t:options.auto" },
  { "value": "small", "label": "t:options.small" },
  { "value": "medium", "label": "t:options.medium" },
  { "value": "large", "label": "t:options.large" }
]}
```

### Conditional Visibility

```json
{
  "type": "range",
  "id": "custom_width",
  "visible_if": "{{ block.settings.width == 'custom' }}"
}
```

### Block Type References in Schemas

- `"@theme"` — any theme block
- `"@app"` — app blocks
- Named types (e.g., `"text"`, `"button"`) — specific blocks
- `_` prefixed (e.g., `"_divider"`) — internal blocks

### Translation Keys Pattern

All labels use `t:` prefix:
- `t:names.*` — block/section names
- `t:settings.*` — setting labels
- `t:options.*` — option labels
- `t:content.*` — section headers
- `t:categories.*` — preset categories
- `t:info.*` — info text

---

## CSS Standards

### Architecture

- Component CSS in `{% stylesheet %}` tags (scoped, deduplicated by Shopify)
- Global/shared CSS in `assets/base.css`
- CSS custom properties for all theming (no hardcoded values)
- No CSS preprocessors (Sass, Less) — use native CSS

### Naming

- BEM convention: `.block`, `.block__element`, `.block--modifier`
- Utility classes: `.spacing-style`, `.size-style`, `.layout-panel-flex`
- Width classes: `.section--page-width`, `.section--full-width`
- Direction classes: `.layout-panel-flex--column`, `.layout-panel-flex--row`

### Key Rules

- Use CSS custom properties from `theme-styles-variables.liquid`
- Use `container-type: inline-size` for responsive internal layouts
- Use logical properties (`padding-block-start`, `margin-inline-end`)
- Color scheme via `.color-{scheme-id}` classes, not hardcoded colors
- Responsive: mobile-first approach
- No `!important` unless overriding third-party styles
- No inline styles except via the style snippet system

### Spacing & Layout

```css
/* Use the spacing scale variables */
gap: var(--spacing-md);
padding: var(--spacing-lg);

/* Use layout utility classes */
.layout-panel-flex { display: flex; }
.layout-panel-flex--column { flex-direction: column; }
.layout-panel-flex--row { flex-direction: row; }
```

---

## HTML Standards

### Key Rules

- Semantic HTML5 elements (`<article>`, `<nav>`, `<section>`, `<aside>`, `<header>`, `<footer>`)
- ARIA attributes where needed (see accessibility reference)
- `{{ block.shopify_attributes }}` on every block root element
- `{{ section.shopify_attributes }}` on every section root element (handled by snippets/section.liquid)
- Use `loading="lazy"` on below-fold images
- Use `<button>` for actions, `<a>` for navigation
- No inline event handlers (`onclick`, etc.) — use JS event listeners

---

## JavaScript Standards

### Module Pattern

```javascript
// assets/my-component.js
import { Component } from '@theme/component';

class MyComponent extends Component {
  connectedCallback() {
    super.connectedCallback();
    // setup
  }

  disconnectedCallback() {
    super.disconnectedCallback();
    // cleanup
  }
}

customElements.define('my-component', MyComponent);
```

### Adding to Importmap

In `snippets/scripts.liquid`:
```javascript
"@theme/my-component": "{{ 'my-component.js' | asset_url }}"
```

### Key Rules

- ES modules only — no CommonJS, no bundlers
- Web Components via `@theme/component` base class
- No frameworks (React, Vue, jQuery)
- Native DOM APIs (`querySelector`, `addEventListener`, etc.)
- `defer`/`async` on all script tags
- `fetchpriority="low"` for non-critical components
- Keep total JS < 16KB minified
- Use IIFE for scoping if not using modules
- Events via `@theme/events` module
- Section rendering via `@theme/section-renderer`

---

## Liquid Standards

### Key Rules

- Use `{% render %}` not `{% include %}` (deprecated)
- Use `{% content_for 'blocks' %}` for child block rendering
- Use `{% content_for 'block', type: 'type', id: 'id' %}` for static blocks
- Whitespace control with `{%-` and `-%}` to minimize output
- Pre-process operations before loops (e.g., sort collections before iterating)
- Access settings: `{{ settings.id }}`, `{{ section.settings.id }}`, `{{ block.settings.id }}`
- Translation: `{{ 'key.path' | t }}`
- Asset URLs: `{{ 'file.js' | asset_url }}`
- Image tags: `{{ image | image_url: width: 300 | image_tag }}`
- Use `| default:` filter for fallback values
- Use `blank` check for resource settings: `{% if settings.page != blank %}`

### Filters

Common Shopify-specific filters:
- `| asset_url` — CDN URL for assets
- `| image_url` — CDN URL for images with resizing
- `| image_tag` — generates `<img>` with srcset
- `| stylesheet_tag` — generates `<link>` tag
- `| script_tag` — generates `<script>` tag
- `| money` — format currency
- `| t` — translate
- `| json` — JSON encode
- `| escape` — HTML escape

---

## Locales

### File Structure

```
locales/
  en.default.json          # Default storefront translations
  en.default.schema.json   # Theme editor translations
  fr.json                  # French storefront
  fr.schema.json           # French theme editor
```

### Storefront Locale (`en.default.json`)

```json
{
  "general": {
    "skip_to_content": "Skip to content",
    "social": {
      "links": {
        "facebook": "Facebook"
      }
    }
  },
  "sections": {
    "header": {
      "menu": "Menu",
      "cart": "Cart"
    }
  }
}
```

### Schema Locale (`en.default.schema.json`)

```json
{
  "names": {
    "section": "Section",
    "hero": "Hero"
  },
  "settings": {
    "color_scheme": "Color scheme",
    "content_direction": "Content direction"
  },
  "options": {
    "column": "Column",
    "row": "Row",
    "small": "Small",
    "medium": "Medium",
    "large": "Large"
  },
  "categories": {
    "content": "Content",
    "media": "Media"
  }
}
```

### Key Rules

- Use `t:` prefix in all schema text fields
- Keys organized hierarchically: `t:names.*`, `t:settings.*`, `t:options.*`
- Max 3,400 translations per file
- Translation values max 1,000 characters
- Use descriptive keys for translator context
- Always add translations when adding new settings/sections/blocks

---

## Theme Settings

### settings_schema.json

Defines theme-level settings shown in Theme Settings area of the editor:

```json
[
  {
    "name": "t:settings_schema.colors.name",
    "settings": [
      {
        "type": "color_scheme_group",
        "id": "color_schemes",
        "definition": [...]
      }
    ]
  },
  {
    "name": "t:settings_schema.typography.name",
    "settings": [
      {
        "type": "font_picker",
        "id": "type_body_font",
        "default": "assistant_n4",
        "label": "t:settings_schema.typography.settings.body_font.label"
      }
    ]
  }
]
```

### settings_data.json

Stores saved values. Auto-generated — do not edit directly except for initial theme configuration.

### Accessing Settings

```liquid
{{ settings.color_schemes }}     {# global #}
{{ section.settings.gap }}       {# section-level #}
{{ block.settings.text }}        {# block-level #}
```
