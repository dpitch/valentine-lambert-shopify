# Stretch Theme — Sections & Blocks Reference

> Stretch has 30+ customizable sections. This reference covers the key sections,
> their purpose, and configuration patterns.

## Layout & Structure Sections

### Dynamic Grid
- **Exclusive to Stretch** — Foundation for all page layouts
- Desktop: 16 columns | Mobile: 8 columns
- Guidelines visible in editor only
- Each block gets: column width, row position, device visibility (both/desktop/mobile)
- Setting: "Allow transparent header" (when first section)
- Layer order: first-added = on top (place text/images above videos)
- Blocks: Image, Text, Video, Product, Collection, Custom HTML

### Header
- Sticky header support
- Transparent header mode (requires compatible first section)
- Mega-menu support (add mega-menu blocks)
- Logo + transparent logo (sizes must match exactly)
- Navigation, search, cart icon, account link
- In-menu promotions

### Footer
- Multi-column layout
- Newsletter signup
- Social links
- Payment icons
- Copyright

### Section Groups
- `header-group.json` — Header sections
- `footer-group.json` — Footer sections

## Content Sections

### Slideshow
- Full-width image/video slides
- Supports transparent header overlay
- Auto-play, transition effects
- Content overlay with text + CTA buttons

### Image with Text Overlay
- Full-width image with text positioned over it
- Supports transparent header
- Heading with effects (italic highlight)
- CTA button

### Image with Text
- Side-by-side layout (image + text block)
- Reversible layout
- Multiple content blocks

### Rich Text
- Heading with highlight effects
- Body text (rich text editor)
- Buttons
- Custom spacing

### Featured Collection
- Display products from a collection
- Grid layout with configurable columns
- Product card highlighting (via metafield)
- Image blocks alongside products

### Collection List
- Display multiple collections as cards
- Grid layout
- Image + title per collection

### Featured Product
- Showcase a single product
- Gallery, variant picker, add to cart
- Multiple info blocks

### Product Recommendations
- AI-powered product suggestions
- Grid layout matching collection pages

### Recently Viewed
- Products the customer has browsed
- Automatic tracking

## Interactive Sections

### Before/After Slider
- Two images with a draggable comparison slider
- Great for transformations, comparisons

### Image Hotspot
- Image with clickable hotspot markers
- Each hotspot links to a product or content
- Tooltip on hover

### Lookbook
- Full-page editorial layouts
- Multiple images with product tags
- Magazine-style presentation

### Countdown Timer
- Date/time target
- Custom messaging for expired timers
- Urgency and promotional use

### FAQ / Accordion
- Collapsible question/answer pairs
- Multiple FAQ groups
- Schema.org FAQ markup

## Product Page Sections

### Product Page (main)
- ~30 blocks available within this section
- Gallery (zoom, rollover, lightbox via PhotoSwipe)
- Variant picker (dropdowns or buttons)
- Color swatches (native or config-based)
- Size chart (global or per-product via metafield)
- Custom badges (via `custom.badges` metafield, list of text values)
- Product tabs
- Usage/shipping/nutritional information blocks
- Price, quantity, add to cart
- Trust badges
- Stock counter
- Share buttons

### Product Tabs
- Organize product info in tabbed interface
- Description, specifications, reviews, etc.

## Collection Page Sections

### Collection Banner
- Full-width collection image (2400x900px recommended)
- Supports transparent header
- Collection title + description

### Product Grid
- Filterable, sortable product grid
- Swatch filters
- Product card highlighting (larger cards for highlighted products)
- Configurable columns

## Marketing Sections

### Promotional Banner
- Top-of-page announcement bar
- Dismissible
- Link support

### Promotional Popup
- Modal popup with timing controls
- Email capture
- Frequency controls

### Promotional Tiles
- Grid of promotional cards
- Image + text + link per tile

### Press Coverage
- Logo bar of press mentions
- Quotes or brand logos

### Trust Badges
- Icon + text badges
- Shipping, returns, security messaging

### Newsletter
- Email signup form
- Integration with Shopify marketing

### Blog Posts
- Display recent blog posts
- Grid or list layout
- Image 2400x1200px recommended

## Utility Sections

### Custom HTML
- Raw HTML/Liquid insertion
- Use sparingly

### Page Content
- For static pages (About, Contact, etc.)
- Rich text rendering

### Contact Form
- Customizable fields
- Shopify-native form handling

### Age Verifier
- Age gate popup
- Configurable messaging

### Search
- Enhanced search with suggestions
- Product, collection, page results

## Block Patterns

### Common Block Settings
Most blocks share these setting patterns:
- **Heading**: Text input with heading level selector (H1-H6)
- **Heading effect**: Override global heading highlight effect per section
- **Content alignment**: Left, center, right
- **Color scheme**: Inherit or override section color scheme
- **Padding/spacing**: Top and bottom padding controls
- **Visibility**: Show on desktop, mobile, or both

### Product Card Blocks
- Product image (aspect ratio, hover effect)
- Product title
- Price (sale price styling)
- Vendor name
- Color swatches (on card)
- Quick buy button
- Badge display

## Template JSON Structure

Templates are JSON files in `templates/` that define which sections appear on each page type:

```json
{
  "sections": {
    "main": {
      "type": "product",
      "blocks": {
        "title": { "type": "title" },
        "price": { "type": "price" },
        "variant_picker": { "type": "variant_picker" },
        "buy_buttons": { "type": "buy_buttons" }
      },
      "block_order": ["title", "price", "variant_picker", "buy_buttons"]
    },
    "recommendations": {
      "type": "product-recommendations"
    }
  },
  "order": ["main", "recommendations"]
}
```

### Available Template Types
- `index.json` — Home page
- `product.json` — Product pages
- `collection.json` — Collection pages
- `list-collections.json` — All collections
- `blog.json` — Blog listing
- `article.json` — Blog post
- `page.json` — Static pages
- `cart.json` — Cart page
- `search.json` — Search results
- `404.json` — Not found
- `password.json` — Password page
- `gift_card.liquid` — Gift card (Liquid, not JSON)
