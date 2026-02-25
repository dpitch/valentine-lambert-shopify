# Stretch Theme — Configuration Guide

Complete guide for configuring all major Stretch theme features.

## Mega-Menu

### Setup
1. Create a 3-level menu hierarchy in Shopify admin (e.g., "Shop" > "Bags" > "Shoulder bags")
2. Theme editor > Header section > "Add mega-menu"
3. Set "Menu item" — **must match exactly** the first-level link name
4. Optionally add up to 2 promotional images

### Best Practices
- Use for large navigation with multiple columns
- Don't use for simple navigation (use standard dropdowns)
- Cap at 4-5 columns per mega-menu
- For multi-language: use Translate & Adapt app, then localize mega-menu block names

## Color Swatches

### Native Swatches (Recommended for newer versions)
- Uses Shopify category metafields
- Managed directly from Shopify Admin
- No complex code setup

### Config-Based Swatches
1. **Color mapping**: Theme editor > "Color swatch" section
   - Hex: `Red:#ff0000` or `Buffalo Brown:#6b483e`
   - Image: Create 100x100px JPG, upload to Files, use `Zebra:zebra.jpg`
2. **Option names**: Edit default theme content > filter "Size" > add comma-separated names like "Color, Material, Finish"
3. **Product pages**: Product section > Variant Picker block > "Color selector type" = "Color swatch"
4. **Collection pages**: Theme Settings > Product Card > "Color display mode" = "Swatch"

### Multi-Language Swatches
Translate color names but keep hex/filenames: `Red:#ff0000` becomes `Rouge:#ff0000`

### Limitations
- One swatch per color name
- 20+ colors may impact collection page performance
- Alternative: use variant images

## Transparent Header

### Compatible Sections (ONLY these work)
- Slideshow
- Image with text overlay
- Collection banner (collection pages only)
- Dynamic Grid (when first section)

### Setup
1. Add compatible section as first section on page
2. In that section's settings: toggle "Allow transparent header"
3. Header section > Transparent Header settings:
   - Upload transparent logo
   - Set text color for transparent state

### Critical Rule
**Transparent logo size must exactly match non-transparent logo size** — mismatched dimensions cause poor transitions.

## Product Pages

### Overview
- ~30 blocks available in the Product Page section
- Changes to default template apply to ALL products
- Use alternate templates for product-specific layouts

### Key Blocks
- **Title** — Product title
- **Price** — Current/compare-at price
- **Variant Picker** — Dropdowns or buttons, color swatches
- **Buy Buttons** — Add to cart, buy now
- **Description** — Product description
- **Gallery** — Product images with zoom/lightbox
- **Badges** — Custom badges via metafield
- **Size Chart** — Global or per-product
- **Tabs** — Organize info in tabs
- **Stock Counter** — Show remaining inventory
- **Trust Badges** — Shipping/returns/security
- **Share** — Social share buttons
- **Usage Info** — Product usage details
- **Shipping Info** — Delivery information
- **Nutritional Info** — For food/supplement products

### Gallery Options
- Image zoom on hover (rollover)
- Pinch-to-zoom on mobile (PhotoSwipe)
- Lightbox fullscreen view
- Video support
- Variant image switching

## Size Charts

### Requirements
- Product must have a variant option named **"Size"** (auto-localized: "Taille" in FR, etc.)
- Products without variants cannot use this feature

### Global Size Chart (all products)
1. Create a Shopify page with size info
2. Theme editor > Product section > "Variant picker" block
3. Select page in "Size chart" setting

### Per-Product Size Charts
1. Settings > Custom data > Products > Add definition
   - Type: "Page reference" > "One page"
2. Theme editor > Variant picker > click database icon next to "Size chart"
3. On each product: Metafields > select the size chart page

### Custom Trigger Words
Edit default theme content > filter "Size" > add alternatives like "Dimensions, Width"

## Custom Badges

### Setup
1. Settings > Custom data > Products > Add definition:
   - Name: `Badges`
   - Type: `Single-line text`
   - Format: `List of values`
   - Optional: restrict to approved values
2. Per product: Metafields > enter badge text values
3. Theme editor > Product page > Add "Badges" block

Badges display alongside built-in "On sale" and "Sold out" indicators.

## Variant Images

### Variant Image Sets (show different images per variant)
1. Choose controlling option (typically "Color")
2. Edit Alt tags on product images
3. Format: `#option_value` (e.g., `#color_black`, `#color_deep blue`)
4. Untagged images (no #) show for all variants

**Limitations**: Single option only, doesn't work with multi-language stores

### Variant Image for Color Options
1. Theme editor > Product > Variant Picker block
2. "Show variant image for options" > enter comma-separated option names
3. All variants MUST have assigned images

## Product Variations (Linked Products)

For products too different for standard Shopify variants (e.g., completely different images/descriptions per color).

### Setup
1. Create individual products for each variation
2. Create metafields:
   - `custom.variation_value` — Single-line text (e.g., "Red", "Blue")
   - Product reference list — All related products
3. Theme editor: map option name + metafields
4. Per product: fill variation value + select all related products

**Limitation**: Cannot combine with native color swatches

## Product Card Highlighting (Collection Pages)

### Metafield Setup
1. Settings > Custom data > Products > Add definition:
   - Name: `Highlight Card`
   - Namespace: `custom`
   - Key: `highlight_card`
   - Type: `True/false`
2. Per product: toggle Highlight Card on
3. Product appears larger in every collection it belongs to

**Best practice**: Use sparingly for new arrivals or featured items. Allow minutes for cache updates.

## Products Filtering

- Managed via Shopify's **Search & Discovery** app (free)
- Filters auto-reflect in theme
- Color swatches activate automatically
- Supports category metafield filters + native swatches
- Collections 5000+ products: Shopify hides filters automatically
- Price filtering: primary currency only

## Heading Effects

### Global Default
Theme Settings > "Heading effect" — applies to all sections

### Per-Section Override
Each section with headings has its own "Heading effect" setting

### Applying Effects
1. Edit heading text in section
2. Select word(s)
3. Apply "Italic" formatting
4. The italic text gets the chosen effect style

### 9 Effect Options
None, Default, Italic Font Override, Circle, Circle (Pencil), Underline, Underline (Pencil), Marker, Tilted Marker

## Metafields & Metaobjects

### Creating Metafields
1. Settings > Custom data > select resource type
2. Add definition (name, namespace, key, type)
3. Fill values per resource
4. Connect in theme editor via database icon

### Type Matching
Metafield type must match setting type (image <> image, text <> text, etc.)

### Limitations
- PDF metafields cannot connect via theme editor
- Variant metafields require developer work
- Metaobjects are groups of metafields (advanced, rarely needed)

## Image Size Recommendations

| Context | Dimensions | Format |
|---------|-----------|--------|
| Product (square) | 2400 x 2400px | JPG |
| Product (portrait) | 1800 x 2400px | JPG |
| Collection banner | 2400 x 900px | JPG |
| Blog post | 2400 x 1200px | JPG |

- Always use JPG for speed (avoid GIF, use PNG only if transparency needed)
- Keep dimensions consistent across products
- Theme editor shows recommended sizes below each image picker

## Alternate Templates

### Creating
1. Theme editor > page selector > "Create template"
2. Base on existing template or default
3. Name appears in editor (spaces → hyphens)

### Assigning
1. Shopify admin > resource (product/page/collection)
2. "Online store" card > select template
3. Only works with published themes

### Limitations
- Cannot create alternate home page templates
- Max 1000 templates
- Deletion requires code editing
- No performance impact from many templates
