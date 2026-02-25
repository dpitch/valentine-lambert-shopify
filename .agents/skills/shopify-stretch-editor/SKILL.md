---
name: shopify-stretch-editor
description: >
  Expert-level editing of the Shopify Stretch theme by Maestrooo. Use when customizing, extending,
  fixing, or creating sections, blocks, snippets, templates, or settings in a Stretch theme.
  Triggers on: "Shopify theme", "Stretch theme", "Liquid section", "theme block", "Shopify section",
  "shopify template", "theme editor", "Shopify CSS", "Shopify JS", "dynamic grid", "heading effect",
  "product highlight", editing .liquid files inside sections/, snippets/, layout/, or templates/
  directories, editing settings_schema.json, or any Shopify storefront theme development work.
---

# Shopify Stretch Theme Editor

> **Theme**: Stretch v1.13.0 by Maestrooo (Paris, FR)
> **Price**: $400 USD | **Presets**: Stretch, Snow, Diffuse
> **Doc**: https://support.maestrooo.com/collection/678-getting-started
> **Stretch-specific**: https://support.maestrooo.com/category/744-stretch

## Quick Orientation

Stretch is a **full-width oriented theme** with 30+ customizable sections, subtle animations, bold text, and fullwidth visuals. It uses a **dynamic grid system** (16-col desktop / 8-col mobile) as its layout foundation.

### File Structure

```
layout/           # theme.liquid (master HTML shell), password.liquid
templates/        # JSON page templates (index, product, collection, etc.)
sections/         # Section Liquid files + section group JSON
snippets/         # Rendering helpers (css-variables.liquid, image.liquid, meta-tags.liquid)
assets/           # theme.css (single file), theme.js, vendor.min.js, custom.css/js
config/           # settings_schema.json, settings_data.json
locales/          # Translation JSON files (fr, en, it, de, es)
```

### Key Differences from Other Themes

- **No `blocks/` directory** — Stretch does NOT use a separate blocks folder (unlike Horizon)
- **Single CSS file** — `theme.css` is consolidated, NOT split across multiple files
- **Single JS file** — `theme.js` bundles all generic components + section code
- **Web components** throughout — behavior via custom elements, no frameworks
- **CSS variables** defined in `snippets/css-variables.liquid`
- **Dynamic Grid** — unique 16-column grid system exclusive to Stretch

## Core Architecture

### CSS Architecture

- **Single `theme.css`** — All CSS in one file for optimal GZip compression
- **No CSS framework** — Handcrafted CSS using native grid/flexbox
- **CSS variables** exposed in `snippets/css-variables.liquid`, configurable via theme editor
- **Per-section CSS variables** defined at top of each section, reducing duplication
- **Custom CSS**: Minor edits use the "Custom CSS" box in theme editor; major changes go in a separate `custom.css` linked in `<head>`

### JavaScript Architecture

- **Web components** — All interactive behavior via custom elements
- **ES modules** via `vendor.min.js` (dependencies) + `theme.js` (components + sections)
- **Module imports**: `<script type="module">import {X} from "vendor";</script>`
- **Custom JS**: Create `custom.js` linked in `<head>`, NEVER edit `theme.js` or `vendor.min.js`
- **Extending components**: Import and extend, e.g. `import {Drawer} from "theme"; class CustomDrawer extends Drawer {}`

### Dependencies (as of v1.13.0)

| Library | Version | Purpose |
|---------|---------|---------|
| Motion | v12.4.7 | Animations (WebAnimations API) |
| ftdomdelegate | v5.0.0 | Event delegation |
| focustrap | v7.6.4 | Focus trapping (accessibility) |
| PhotoSwipe | v5.2.3 | Pinch-to-zoom / lightbox galleries |
| custom-elements polyfill | v1.1.0 | Safari support (conditional) |

> Motion v12 API differs from v10 (used in Prestige/Impact). Available exports: `animate`, `timeline`, `inView`, `scroll`, `ScrollOffset`.

## Stretch-Exclusive Features

### 1. Dynamic Grid

The signature feature. Creates professional layouts via an invisible column structure.

- **Desktop**: 16 columns | **Mobile**: 8 columns
- Guidelines visible in theme editor only (invisible to customers)
- Each content block gets a column width assignment
- **Visibility per device**: both, desktop-only, or mobile-only
- **Transparent header**: "Allow transparent header" option when grid is first section
- **Layer order**: Elements added first appear on top — place text/images above videos

**Best practices**:
- Use wide spans (10+ cols) for featured content, narrow (2-3 cols) for supporting elements
- Always preview on both desktop and mobile
- Column widths create visual hierarchy

### 2. Heading Effects

Apply visual emphasis to specific words in headings using italic formatting.

**How to apply**:
1. Navigate to any section with a heading
2. Click heading text to edit
3. Select word(s) to highlight
4. Choose "Italic" from formatting options

**Available effects** (9 options):
- `None` — No effect
- `Default` — Uses global Theme Settings > Heading effect
- `Italic Font Override` — Different font for highlighted text
- `Circle` — Solid circle around words
- `Circle (Pencil)` — Hand-drawn circle
- `Underline` — Standard underline
- `Underline (Pencil)` — Hand-drawn underline
- `Marker` — Marker highlight
- `Tilted Marker` — Angled marker

**Best practices**: Highlight 1-2 key words max. Don't highlight entire phrases. Don't use on H3+. Keep consistent across similar sections.

### 3. Product Card Highlighting

Make specific products larger in collection pages.

**Setup** — Create a metafield:
1. Settings > Custom data > Products > Add definition
2. Name: `Highlight Card` | Namespace: `custom` | Key: `highlight_card` | Type: `True/false`
3. On each product: Metafields > toggle Highlight Card on
4. Use sparingly — new arrivals, featured items only

## Feature Reference

### Cart & Checkout
Cart notes, in-store pickups, pre-order, quick buy, Shop sign-in, slide-out cart, sticky cart

### Marketing & Conversion
Trust badges, stock counter, RTL support, product recommendations, recently viewed, quick view, promo tiles/popups/banners, product badges, press coverage, in-menu promos, FAQ page, EU translations (EN/FR/IT/DE/ES), customizable contact form, cross-selling, countdown timer, blogs, age verifier, quantity pricing (Plus)

### Merchandising
Usage info, slideshow, size chart, shipping info, product videos/tabs/options, lookbooks, nutritional info, image zoom/rollover/hotspot/galleries, high-res images, color swatches, before/after slider, animations, combined listing (Plus)

### Product Discovery
Collection navigation, enhanced search, mega menu, filtering/sorting, recently viewed, recommendations, sticky header, swatch filters

## Reference Files

- **[references/sections-and-blocks.md](references/sections-and-blocks.md)** — All 30+ sections, block types, settings for each section. Read when creating or editing sections.
- **[references/configuration-guide.md](references/configuration-guide.md)** — Mega-menu, color swatches, transparent header, product pages, filtering, size charts, badges, variant images, metafields. Read when configuring theme features.
- **[references/javascript-events.md](references/javascript-events.md)** — All custom JS events, payloads, cart/product/UI events. Read when adding custom JavaScript behavior.

## Workflows

### Customizing via Theme Editor (preferred)

1. Open Shopify admin > Themes > Customize
2. Add/reorder/configure sections and blocks visually
3. Use "Custom CSS" box for minor CSS adjustments
4. Save and preview on both desktop and mobile

### Adding Custom Code

1. **CSS**: Create `assets/custom.css`, link in `layout/theme.liquid` `<head>`
2. **JS**: Create `assets/custom.js`, link in `layout/theme.liquid` `<head>`
3. **NEVER** edit `theme.css`, `theme.js`, or `vendor.min.js` — breaks auto-updates

### Creating Custom Sections

1. Create `sections/my-section.liquid`
2. Write Liquid + HTML + schema
3. Add `{% schema %}` with settings, blocks, presets
4. Add translation keys to locale files
5. Reference in template JSON file

### Creating Alternate Templates

1. Theme editor > page selector > "Create template"
2. Can base on existing template
3. Assign to specific products/pages/collections in Shopify admin
4. Max 1000 templates, no performance impact
5. Cannot create alternate home page templates

### Using Metafields

1. Settings > Custom data > Add definition (name, namespace, key, type)
2. Fill data on each resource (product, collection, page)
3. In theme editor: click database icon next to a setting to connect metafield
4. Types must match (image metafield <> image setting)
5. Variant metafields require developer work

### Product Variations (Linked Products)

For products too different for Shopify variants:
1. Create individual products per variation
2. Create metafields: `custom.variation_value` (text) + product reference list
3. Map in theme editor: option name + metafields
4. Cannot combine with native color swatches

## Important Rules

1. **NEVER edit core files** (`theme.css`, `theme.js`, `vendor.min.js`) — use `custom.css`/`custom.js`
2. **NEVER use `shopify theme push/pull`** if GitHub sync is configured — use `git push origin main`
3. **Always `git pull` first** to get changes from Shopify editor
4. **Test on both desktop and mobile** — Dynamic Grid behaves differently
5. **Image recommendations**: Products 2400x2400 (square) or 1800x2400 (portrait) JPG; Collections 2400x900; Blog 2400x1200
6. **Commit messages in French**: `feat:`, `fix:`, `style:`, `refactor:`, `docs:`, `chore:`
