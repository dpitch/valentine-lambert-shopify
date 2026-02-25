# Shopify Theme Settings Reference

All available input setting types for use in section/block schemas and `settings_schema.json`.

## Table of Contents

1. [Basic Input Settings](#basic-input-settings)
2. [Specialized Input Settings](#specialized-input-settings)
3. [Resource Settings](#resource-settings)
4. [Sidebar Settings](#sidebar-settings)
5. [Common Horizon Settings Patterns](#common-horizon-settings-patterns)
6. [Conditional Visibility](#conditional-visibility)
7. [Dynamic Sources](#dynamic-sources)

---

## Basic Input Settings

### checkbox
Returns `Boolean`. Default: `false`.
```json
{ "type": "checkbox", "id": "show_title", "label": "t:settings.show_title", "default": true }
```

### number
Returns `Number` or `nil`. Default must be numeric (not string).
```json
{ "type": "number", "id": "items_count", "label": "t:settings.items_count", "default": 4 }
```

### radio
Returns `String`. Requires `options` array.
```json
{
  "type": "radio",
  "id": "layout",
  "label": "t:settings.layout",
  "options": [
    { "value": "grid", "label": "t:options.grid" },
    { "value": "list", "label": "t:options.list" }
  ],
  "default": "grid"
}
```

### range
Returns `Number`. Requires `min`, `max`, `step`, `default`.
```json
{ "type": "range", "id": "columns", "min": 1, "max": 6, "step": 1, "default": 4, "label": "t:settings.columns" }
```

### select
Returns `String`. Requires `options` array.
```json
{
  "type": "select",
  "id": "size",
  "label": "t:settings.size",
  "options": [
    { "value": "small", "label": "t:options.small" },
    { "value": "medium", "label": "t:options.medium" },
    { "value": "large", "label": "t:options.large" }
  ],
  "default": "medium"
}
```

### text
Returns `String` or empty object. Optional `placeholder`.
```json
{ "type": "text", "id": "heading", "label": "t:settings.heading", "default": "Welcome" }
```

### textarea
Returns `String` or empty object. For larger text blocks.
```json
{ "type": "textarea", "id": "description", "label": "t:settings.description" }
```

---

## Specialized Input Settings

### color
Returns color string or blank.
```json
{ "type": "color", "id": "text_color", "label": "t:settings.text_color", "default": "#000000" }
```

### color_background
Returns CSS background color string or blank.
```json
{ "type": "color_background", "id": "bg_gradient", "label": "t:settings.background" }
```

### color_scheme
Returns `String` referencing a predefined color scheme.
```json
{ "type": "color_scheme", "id": "color_scheme", "label": "t:settings.color_scheme", "default": "scheme-1" }
```

### color_scheme_group
Returns `String`. Groups multiple color schemes.
```json
{ "type": "color_scheme_group", "id": "color_schemes", "definition": [...] }
```

### font_picker
Returns font object.
```json
{ "type": "font_picker", "id": "heading_font", "label": "t:settings.heading_font", "default": "assistant_n4" }
```

### html
Returns `String` of raw HTML.
```json
{ "type": "html", "id": "custom_html", "label": "t:settings.custom_html" }
```

### image_picker
Returns image object or blank.
```json
{ "type": "image_picker", "id": "image", "label": "t:settings.image" }
```

### inline_richtext
Returns `String`. Rich text without block-level formatting (bold, italic, links only).
```json
{ "type": "inline_richtext", "id": "text", "label": "t:settings.text", "default": "Some <em>rich</em> text" }
```

### link_list
Returns link list (menu) object or blank.
```json
{ "type": "link_list", "id": "menu", "label": "t:settings.menu", "default": "main-menu" }
```

### liquid
Returns `String` of Liquid code.
```json
{ "type": "liquid", "id": "custom_liquid", "label": "t:settings.custom_liquid" }
```

### richtext
Returns `String`. Rich text with block formatting (paragraphs, lists, headings).
```json
{ "type": "richtext", "id": "description", "label": "t:settings.description" }
```

### text_alignment
Returns `String` (left/center/right).
```json
{ "type": "text_alignment", "id": "alignment", "label": "t:settings.alignment", "default": "center" }
```

### url
Returns `String` or blank.
```json
{ "type": "url", "id": "link", "label": "t:settings.link" }
```

### video
Returns video object or blank.
```json
{ "type": "video", "id": "video", "label": "t:settings.video" }
```

### video_url
Returns `String` for external video URLs (YouTube, Vimeo).
```json
{ "type": "video_url", "id": "video_url", "label": "t:settings.video_url", "accept": ["youtube", "vimeo"] }
```

---

## Resource Settings

These return Shopify resource objects. May return blank if resource doesn't exist.

### article / article_list
```json
{ "type": "article", "id": "article", "label": "t:settings.article" }
{ "type": "article_list", "id": "articles", "label": "t:settings.articles", "limit": 10 }
```

### blog
```json
{ "type": "blog", "id": "blog", "label": "t:settings.blog" }
```

### collection / collection_list
```json
{ "type": "collection", "id": "collection", "label": "t:settings.collection" }
{ "type": "collection_list", "id": "collections", "label": "t:settings.collections", "limit": 15 }
```

### metaobject / metaobject_list
```json
{ "type": "metaobject", "id": "custom_data", "label": "t:settings.custom_data" }
{ "type": "metaobject_list", "id": "custom_items", "label": "t:settings.custom_items" }
```

### page
```json
{ "type": "page", "id": "page", "label": "t:settings.page" }
```

### product / product_list
```json
{ "type": "product", "id": "product", "label": "t:settings.product" }
{ "type": "product_list", "id": "products", "label": "t:settings.products", "limit": 12 }
```

### Checking Resource Settings in Liquid

Always check for blank:
```liquid
{% if section.settings.collection != blank %}
  {% for product in section.settings.collection.products %}
    ...
  {% endfor %}
{% endif %}
```

---

## Sidebar Settings

Non-configurable informational elements (no stored value):

### header
```json
{ "type": "header", "content": "t:content.layout_settings" }
```

### paragraph
```json
{ "type": "paragraph", "content": "t:info.layout_description" }
```

---

## Common Horizon Settings Patterns

### Layout Group
```json
{ "type": "header", "content": "t:content.layout" },
{
  "type": "select", "id": "content_direction",
  "label": "t:settings.content_direction",
  "options": [
    { "value": "column", "label": "t:options.column" },
    { "value": "row", "label": "t:options.row" }
  ],
  "default": "column"
},
{
  "type": "select", "id": "horizontal_alignment",
  "label": "t:settings.horizontal_alignment",
  "options": [
    { "value": "start", "label": "t:options.start" },
    { "value": "center", "label": "t:options.center" },
    { "value": "end", "label": "t:options.end" },
    { "value": "space-between", "label": "t:options.space_between" }
  ],
  "default": "start"
},
{
  "type": "select", "id": "vertical_alignment",
  "label": "t:settings.vertical_alignment",
  "options": [
    { "value": "start", "label": "t:options.start" },
    { "value": "center", "label": "t:options.center" },
    { "value": "end", "label": "t:options.end" }
  ],
  "default": "start"
},
{
  "type": "select", "id": "gap",
  "label": "t:settings.gap",
  "options": [
    { "value": "none", "label": "t:options.none" },
    { "value": "small", "label": "t:options.small" },
    { "value": "medium", "label": "t:options.medium" },
    { "value": "large", "label": "t:options.large" }
  ],
  "default": "medium"
}
```

### Size Group
```json
{ "type": "header", "content": "t:content.size" },
{
  "type": "select", "id": "section_width",
  "label": "t:settings.section_width",
  "options": [
    { "value": "page-width", "label": "t:options.page_width" },
    { "value": "full-width", "label": "t:options.full_width" }
  ],
  "default": "page-width"
},
{
  "type": "select", "id": "section_height",
  "label": "t:settings.section_height",
  "options": [
    { "value": "auto", "label": "t:options.auto" },
    { "value": "small", "label": "t:options.small" },
    { "value": "medium", "label": "t:options.medium" },
    { "value": "large", "label": "t:options.large" },
    { "value": "screen", "label": "t:options.screen" }
  ],
  "default": "auto"
}
```

### Appearance Group
```json
{ "type": "header", "content": "t:content.appearance" },
{ "type": "color_scheme", "id": "color_scheme", "label": "t:settings.color_scheme" },
{
  "type": "select", "id": "background_media",
  "label": "t:settings.background_media",
  "options": [
    { "value": "none", "label": "t:options.none" },
    { "value": "image", "label": "t:options.image" },
    { "value": "video", "label": "t:options.video" }
  ],
  "default": "none"
},
{ "type": "image_picker", "id": "background_image", "label": "t:settings.background_image",
  "visible_if": "{{ section.settings.background_media == 'image' }}" },
{ "type": "video", "id": "background_video", "label": "t:settings.background_video",
  "visible_if": "{{ section.settings.background_media == 'video' }}" },
{ "type": "checkbox", "id": "toggle_overlay", "label": "t:settings.toggle_overlay", "default": false }
```

### Padding Group
```json
{ "type": "header", "content": "t:content.padding" },
{
  "type": "select", "id": "padding-block-start",
  "label": "t:settings.padding_top",
  "options": [
    { "value": "none", "label": "t:options.none" },
    { "value": "small", "label": "t:options.small" },
    { "value": "medium", "label": "t:options.medium" },
    { "value": "large", "label": "t:options.large" }
  ],
  "default": "medium"
},
{
  "type": "select", "id": "padding-block-end",
  "label": "t:settings.padding_bottom",
  "options": [
    { "value": "none", "label": "t:options.none" },
    { "value": "small", "label": "t:options.small" },
    { "value": "medium", "label": "t:options.medium" },
    { "value": "large", "label": "t:options.large" }
  ],
  "default": "medium"
}
```

---

## Conditional Visibility

Use `visible_if` to show/hide settings conditionally:

```json
{
  "type": "range",
  "id": "custom_width",
  "label": "t:settings.custom_width",
  "min": 100,
  "max": 1200,
  "step": 10,
  "default": 400,
  "visible_if": "{{ block.settings.width == 'custom' }}"
}
```

Supported operators: `==`, `!=`, `and`, `or`.

```json
"visible_if": "{{ section.settings.content_direction == 'row' }}"
"visible_if": "{{ block.settings.background_media == 'image' }}"
"visible_if": "{{ block.settings.type_preset == 'custom' }}"
```

---

## Dynamic Sources

Section/block settings in JSON templates can connect to dynamic sources:

```json
{
  "type": "text",
  "settings": {
    "text": "{{ closest.product.title }}"
  }
}
```

The `{{ closest.* }}` syntax accesses the nearest ancestor's context (product, collection, article, etc.).
