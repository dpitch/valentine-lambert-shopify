# Horizon Theme Architecture

Deep dive into how the Horizon theme components connect and render.

## Table of Contents

1. [Rendering Pipeline](#rendering-pipeline)
2. [Layout Layer](#layout-layer)
3. [Section System](#section-system)
4. [Block System](#block-system)
5. [Snippet System](#snippet-system)
6. [Template JSON Format](#template-json-format)
7. [CSS Architecture](#css-architecture)
8. [JavaScript Architecture](#javascript-architecture)
9. [File Inventory](#file-inventory)

---

## Rendering Pipeline

```
Request → Layout (theme.liquid)
            ├─ <head>: meta-tags, stylesheets, fonts, scripts, theme-styles-variables, color-schemes
            ├─ <body>:
            │   ├─ {% sections 'header-group' %} → header-announcements + header sections
            │   ├─ <main> {{ content_for_layout }} → Template JSON
            │   │   └─ Sections (from template "sections" + "order")
            │   │       └─ Blocks (from section "blocks" + "block_order")
            │   │           └─ Nested blocks (up to 8 levels)
            │   └─ {% sections 'footer-group' %} → footer + footer-utilities sections
            └─ Cart drawer, search modal, skip links
```

## Layout Layer

### theme.liquid

The `<head>` renders snippets in this order:
1. `meta-tags` — SEO meta tags
2. `stylesheets` — loads `base.css`, preloads `overflow-list.css`
3. `fonts` — font face declarations
4. `scripts` — importmap + ES module scripts
5. `theme-styles-variables` — all CSS custom properties on `:root`
6. `color-schemes` — per-scheme CSS variable overrides

The `<body>` has three zones:
- `#header-group` wrapping `{% sections 'header-group' %}`
- `<main id="MainContent">` wrapping `{{ content_for_layout }}`
- `<footer>` wrapping `{% sections 'footer-group' %}`

An inline `<script>` between header and main calculates `--header-height`, `--header-group-height`, `--top-row-height` CSS variables to prevent layout shift.

### password.liquid

Simplified layout for password-protected stores.

---

## Section System

### Universal Section (`sections/section.liquid`)

This is the most-used section. It accepts any theme block:

```liquid
{% capture children %}
  {% content_for 'blocks' %}
{% endcapture %}
{% render 'section', section: section, children: children %}
```

The `snippets/section.liquid` renders:
1. `.section-background` — full-bleed color scheme background
2. `.section` — width container (`section--page-width` or `section--full-width`)
3. Background media via `{% render 'background-media' %}`
4. `.overlay` layer
5. `.layout-panel-flex` — flex content wrapper (row or column direction)

Schema accepts: `@theme`, `@app`, `_divider` block types.

### Specialized Sections

| Section | Purpose |
|---------|---------|
| `header.liquid` | Main header with logo, menu, actions |
| `header-announcements.liquid` | Announcement bar above header |
| `footer.liquid` | Footer content with menus and blocks |
| `footer-utilities.liquid` | Footer bottom bar (copyright, policies) |
| `hero.liquid` | Hero banner section |
| `carousel.liquid` | Image/content carousel |
| `slideshow.liquid` | Full-width slideshow |
| `layered-slideshow.liquid` | Layered/parallax slideshow |
| `marquee.liquid` | Scrolling marquee text |
| `media-with-content.liquid` | Split media + content layout |
| `collection-links.liquid` | Collection navigation links |
| `collection-list.liquid` | Collection grid |
| `featured-blog-posts.liquid` | Blog post cards |
| `featured-product.liquid` | Single featured product |
| `featured-product-information.liquid` | Detailed featured product info |
| `product-information.liquid` | Main product page section |
| `product-list.liquid` | Product grid/list |
| `product-recommendations.liquid` | Related products |
| `product-hotspots.liquid` | Interactive image hotspots |
| `main-cart.liquid` | Cart page content |
| `main-collection.liquid` | Collection page content |
| `main-blog.liquid` | Blog listing page |
| `main-blog-post.liquid` | Single blog post |
| `main-page.liquid` | Static page content |
| `main-404.liquid` | 404 error page |
| `main-collection-list.liquid` | Collection directory |
| `search-header.liquid` | Search page header |
| `search-results.liquid` | Search results |
| `predictive-search.liquid` | Live search predictions |
| `quick-order-list.liquid` | Bulk ordering interface |
| `custom-liquid.liquid` | Freeform Liquid code section |
| `divider.liquid` | Visual separator |
| `logo.liquid` | Logo display section |

### Section Groups

`header-group.json` and `footer-group.json` define persistent section groups:

```json
{
  "type": "header",
  "name": "t:names.header_group",
  "sections": {
    "header-announcements": { "type": "header-announcements" },
    "header": { "type": "header" }
  },
  "order": ["header-announcements", "header"]
}
```

---

## Block System

### Public Blocks (no prefix)

Available in the theme editor block picker when referenced in schemas:

| Block | Purpose |
|-------|---------|
| `group.liquid` | Flex layout container (row/column) |
| `text.liquid` | Text content with typography settings |
| `image.liquid` | Image display |
| `button.liquid` | CTA button |
| `video.liquid` | Video player |
| `icon.liquid` | Icon display |
| `spacer.liquid` | Vertical/horizontal space |
| `accordion.liquid` | Expandable content |
| `jumbo-text.liquid` | Large display text |
| `comparison-slider.liquid` | Before/after image comparison |
| `email-signup.liquid` | Newsletter signup form |
| `contact-form.liquid` | Contact form |
| `contact-form-submit-button.liquid` | Form submit button |
| `custom-liquid.liquid` | Freeform Liquid |
| `popup-link.liquid` | Modal/popup trigger |
| `collection-card.liquid` | Collection preview card |
| `collection-title.liquid` | Collection heading |
| `product-card.liquid` | Product preview card |
| `product-title.liquid` | Product heading |
| `product-description.liquid` | Product description |
| `product-inventory.liquid` | Stock info |
| `product-custom-property.liquid` | Custom metafield display |
| `product-recommendations.liquid` | Related products |
| `price.liquid` | Price display |
| `variant-picker.liquid` | Variant selection |
| `quantity.liquid` | Quantity selector |
| `buy-buttons.liquid` | Add to cart + checkout |
| `add-to-cart.liquid` | Add to cart button |
| `accelerated-checkout.liquid` | Express checkout buttons |
| `swatches.liquid` | Color/variant swatches |
| `sku.liquid` | SKU display |
| `review.liquid` | Product review |
| `follow-on-shop.liquid` | Follow on Shop button |
| `filters.liquid` | Collection filters |
| `featured-collection.liquid` | Featured collection grid |
| `page.liquid` | Static page content |
| `page-content.liquid` | Page body content |
| `logo.liquid` | Logo |
| `menu.liquid` | Navigation menu |
| `social-links.liquid` | Social media links |
| `payment-icons.liquid` | Payment method icons |
| `footer-copyright.liquid` | Copyright text |
| `footer-policy-list.liquid` | Legal links |

### Private Blocks (`_` prefix)

Internal blocks for specific parent sections:

| Block | Parent Context |
|-------|---------------|
| `_content.liquid` | Simplified group (column-only) |
| `_media.liquid` | Media with appearance settings |
| `_media-without-appearance.liquid` | Media without appearance |
| `_content-without-appearance.liquid` | Content without appearance |
| `_card.liquid` | Card container |
| `_heading.liquid` | Section heading |
| `_inline-text.liquid` | Inline text element |
| `_divider.liquid` | Separator line |
| `_image.liquid` | Internal image variant |
| `_slide.liquid` | Slideshow slide |
| `_layered-slide.liquid` | Layered slideshow slide |
| `_marquee.liquid` | Marquee content |
| `_carousel-content.liquid` | Carousel slide content |
| `_announcement.liquid` | Announcement bar item |
| `_header-logo.liquid` | Header logo |
| `_header-menu.liquid` | Header menu |
| `_search-input.liquid` | Search field |
| `_accordion-row.liquid` | Accordion item |
| `_social-link.liquid` | Single social link |
| `_footer-social-icons.liquid` | Footer social icons |
| `_hotspot-product.liquid` | Product hotspot marker |
| `_product-card.liquid` | Internal product card |
| `_product-card-gallery.liquid` | Product card image gallery |
| `_product-card-group.liquid` | Product card grouping |
| `_product-details.liquid` | Product details panel |
| `_product-media-gallery.liquid` | Product media gallery |
| `_product-list-content.liquid` | Product list header |
| `_product-list-text.liquid` | Product list text |
| `_product-list-button.liquid` | Product list action |
| `_featured-product.liquid` | Featured product layout |
| `_featured-product-gallery.liquid` | Featured product images |
| `_featured-product-price.liquid` | Featured product price |
| `_featured-product-information-carousel.liquid` | Featured product carousel |
| `_collection-card.liquid` | Internal collection card |
| `_collection-card-image.liquid` | Collection card image |
| `_collection-image.liquid` | Collection header image |
| `_collection-info.liquid` | Collection info block |
| `_collection-link.liquid` | Collection link |
| `_inline-collection-title.liquid` | Inline collection heading |
| `_blog-post-card.liquid` | Blog post card |
| `_blog-post-content.liquid` | Blog post content |
| `_blog-post-description.liquid` | Blog post excerpt |
| `_blog-post-featured-image.liquid` | Blog featured image |
| `_blog-post-image.liquid` | Blog post image |
| `_blog-post-info-text.liquid` | Blog post metadata |
| `_featured-blog-posts-card.liquid` | Featured blog card |
| `_featured-blog-posts-image.liquid` | Featured blog image |
| `_featured-blog-posts-title.liquid` | Featured blog title |
| `_cart-title.liquid` | Cart heading |
| `_cart-products.liquid` | Cart items list |
| `_cart-summary.liquid` | Cart totals |

---

## Snippet System

### Rendering Snippets

Core structural snippets:
- `section.liquid` — universal section wrapper
- `group.liquid` — group block renderer
- `background-media.liquid` — background image/video
- `overlay.liquid` — color overlay

### Style Snippets

Inline style generators:
- `spacing-style.liquid` — padding from settings
- `spacing-padding.liquid` — padding helper
- `size-style.liquid` — width/height from settings
- `gap-style.liquid` — gap from settings
- `border-override.liquid` — border CSS
- `layout-panel-style.liquid` — flex alignment/gap
- `typography-style.liquid` — font styling
- `menu-font-styles.liquid` / `submenu-font-styles.liquid`
- `password-layout-styles.liquid`

### Theme Setup Snippets

- `theme-styles-variables.liquid` — CSS custom properties on `:root`
- `color-schemes.liquid` — color scheme CSS classes
- `stylesheets.liquid` — CSS loading
- `scripts.liquid` — JS importmap + module loading
- `fonts.liquid` — font face declarations
- `meta-tags.liquid` — SEO meta tags
- `theme-editor.liquid` — theme editor helpers

### Component Snippets

- `product-card.liquid` — product card rendering
- `collection-card.liquid` — collection card
- `resource-card.liquid` / `resource-image.liquid` / `resource-list.liquid`
- `price.liquid` / `format-price.liquid` / `unit-price.liquid` / `tax-info.liquid`
- `button.liquid` / `add-to-cart-button.liquid`
- `image.liquid` / `media.liquid` / `video.liquid`
- `text.liquid` / `jumbo-text.liquid` / `divider.liquid` / `icon.liquid` / `icon-or-image.liquid`
- `swatch.liquid` / `variant-swatches.liquid` / `variant-main-picker.liquid`
- `quantity-selector.liquid`
- `cart-products.liquid` / `cart-summary.liquid` / `cart-bubble.liquid`
- `product-grid.liquid` / `product-media.liquid` / `product-media-gallery-content.liquid` / `product-information-content.liquid`
- `bento-grid.liquid`
- `slideshow.liquid` / `slideshow-slide.liquid` / `slideshow-arrows.liquid` / `slideshow-arrow.liquid` / `slideshow-controls.liquid`
- `header-row.liquid` / `header-actions.liquid` / `header-drawer.liquid`
- `mega-menu-list.liquid` / `link-featured-image.liquid`
- `search.liquid` / `search-modal.liquid` / `predictive-search-*.liquid`
- `overflow-list.liquid` / `pagination-controls.liquid`
- `sorting.liquid` / `list-filter.liquid` / `filter-remove-buttons.liquid` / `price-filter.liquid` / `grid-density-controls.liquid`
- `localization-form.liquid` / `blog-comment-form.liquid` / `gift-card-recipient-form.liquid`
- `checkbox.liquid` / `skip-to-content-link.liquid` / `quick-add.liquid` / `quick-add-modal.liquid`
- `volume-pricing-info.liquid` / `strikethrough-variant.liquid` / `sku.liquid`
- `editorial-product-grid.liquid` / `editorial-collection-grid.liquid` / `editorial-blog-grid.liquid`

### Utility Snippets

- `util-product-grid-card-size.liquid`
- `util-product-media-sizes-attr.liquid`
- `util-mega-menu-img-sizes-attr.liquid`
- `util-autofill-img-size-attr.liquid`

---

## Template JSON Format

### Product Template Example (simplified)

```json
{
  "sections": {
    "product-information": {
      "type": "product-information",
      "blocks": {
        "media-gallery": {
          "type": "_product-media-gallery",
          "static": true
        },
        "details": {
          "type": "_product-details",
          "static": true,
          "blocks": {
            "header": {
              "type": "group",
              "blocks": {
                "title": {
                  "type": "text",
                  "settings": {
                    "text": "{{ closest.product.title }}"
                  }
                },
                "price": { "type": "price" }
              },
              "block_order": ["title", "price"]
            },
            "divider": { "type": "_divider" },
            "variant-picker": { "type": "variant-picker" },
            "buy-buttons": {
              "type": "buy-buttons",
              "blocks": {
                "quantity": { "type": "quantity", "static": true },
                "add-to-cart": { "type": "add-to-cart", "static": true },
                "checkout": { "type": "accelerated-checkout", "static": true }
              },
              "block_order": ["quantity", "add-to-cart", "checkout"]
            }
          },
          "block_order": ["header", "divider", "variant-picker", "buy-buttons"]
        }
      },
      "block_order": ["media-gallery", "details"]
    }
  },
  "order": ["product-information"]
}
```

Key observations:
- Deep nesting: section → static block → group → leaf blocks
- `{{ closest.product }}` accesses nearest ancestor's product context
- Static blocks (`"static": true`) cannot be moved by merchants
- `block_order` arrays determine rendering sequence

---

## CSS Architecture

### Loading Order

1. `assets/base.css` — global styles (loaded via `stylesheets.liquid`)
2. `{% stylesheet %}` tags — component-scoped CSS (deduplicated by Shopify)
3. `assets/overflow-list.css` — preloaded separately
4. Inline styles via style snippet system

### CSS Custom Properties (`:root`)

Defined in `theme-styles-variables.liquid`:

```css
/* Page widths */
--narrow-page-width: 90rem;
--normal-page-width: 120rem;
--wide-page-width: 150rem;

/* Section heights (responsive) */
--section-height-small: 25rem; /* mobile: 25rem → tablet: 30svh → desktop: 30svh */
--section-height-medium: 35rem;
--section-height-large: 45rem;

/* Z-index layers */
--layer-section-background: -2;
--layer-flat: 0;
--layer-raised: 2;
--layer-sticky: 8;
--layer-overlay: 10;
--layer-temporary: 20;

/* Spacing scale */
--spacing-3xs through --spacing-6xl
```

### Color Scheme CSS

Per-scheme classes (`.color-{id}`) set:
- `--color-background` / `--color-foreground` (+ `-rgb` variants)
- `--color-heading-foreground`
- `--color-primary` / `--color-primary-hover`
- `--color-border` / `--color-shadow`
- Button variables (primary/secondary with text/bg/border/hover)
- Input variables (bg/text/border/hover)
- Adaptive opacity variables based on background brightness

---

## JavaScript Architecture

### Importmap (`snippets/scripts.liquid`)

```javascript
{
  "imports": {
    "@theme/component": "component.js",
    "@theme/utilities": "utilities.js",
    "@theme/events": "events.js",
    "@theme/section-renderer": "section-renderer.js",
    "@theme/section-hydration": "section-hydration.js",
    "@theme/morph": "morph.js",
    "@theme/focus": "focus.js",
    "@theme/scrolling": "scrolling.js",
    // ... ~25+ modules
  }
}
```

### Module Categories

**Core (preloaded):** component, utilities, events, section-renderer, section-hydration, morph, focus, scrolling

**Components (lazy):** header, cart-drawer, slideshow, media-gallery, variant-picker, product-form, facets, predictive-search, quick-add, dialog, etc.

**Template-specific (conditional preload):** paginated-list (collection/search pages)

### Global `Theme` Object

```javascript
window.Theme = {
  translations: { ... },
  routes: {
    cart_url: '/cart',
    cart_add_url: '/cart/add',
    predictive_search_url: '/search/suggest'
  }
};
```
