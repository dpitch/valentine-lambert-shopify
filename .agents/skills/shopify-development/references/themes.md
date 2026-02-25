# Themes Reference

Guide for developing Shopify themes with Liquid templating.

## Liquid Templating

### Syntax Basics

**Objects (Output):**
```liquid
{{ product.title }}
{{ product.price | money }}
{{ customer.email }}
```

**Tags (Logic):**
```liquid
{% if product.available %}
  <button>Add to Cart</button>
{% else %}
  <p>Sold Out</p>
{% endif %}

{% for product in collection.products %}
  {{ product.title }}
{% endfor %}

{% case product.type %}
  {% when 'Clothing' %}
    <span>Apparel</span>
  {% when 'Shoes' %}
    <span>Footwear</span>
  {% else %}
    <span>Other</span>
{% endcase %}
```

**Filters (Transform):**
```liquid
{{ product.title | upcase }}
{{ product.price | money }}
{{ product.description | strip_html | truncate: 100 }}
{{ product.image | img_url: 'medium' }}
{{ 'now' | date: '%B %d, %Y' }}
```

### Common Objects

**Product:**
```liquid
{{ product.id }}
{{ product.title }}
{{ product.handle }}
{{ product.description }}
{{ product.price }}
{{ product.compare_at_price }}
{{ product.available }}
{{ product.type }}
{{ product.vendor }}
{{ product.tags }}
{{ product.images }}
{{ product.variants }}
{{ product.featured_image }}
{{ product.url }}
```

**Collection:**
```liquid
{{ collection.title }}
{{ collection.handle }}
{{ collection.description }}
{{ collection.products }}
{{ collection.products_count }}
{{ collection.image }}
{{ collection.url }}
```

**Cart:**
```liquid
{{ cart.item_count }}
{{ cart.total_price }}
{{ cart.items }}
{{ cart.note }}
{{ cart.attributes }}
```

**Customer:**
```liquid
{{ customer.email }}
{{ customer.first_name }}
{{ customer.last_name }}
{{ customer.orders_count }}
{{ customer.total_spent }}
{{ customer.addresses }}
{{ customer.default_address }}
```

**Shop:**
```liquid
{{ shop.name }}
{{ shop.email }}
{{ shop.domain }}
{{ shop.currency }}
{{ shop.money_format }}
{{ shop.enabled_payment_types }}
```

### Common Filters

**String:**
- `upcase`, `downcase`, `capitalize`
- `strip_html`, `strip_newlines`
- `truncate: 100`, `truncatewords: 20`
- `replace: 'old', 'new'`

**Number:**
- `money` - Format currency
- `round`, `ceil`, `floor`
- `times`, `divided_by`, `plus`, `minus`

**Array:**
- `join: ', '`
- `first`, `last`
- `size`
- `map: 'property'`
- `where: 'property', 'value'`

**URL:**
- `img_url: 'size'` - Image URL
- `url_for_type`, `url_for_vendor`
- `link_to`, `link_to_type`

**Date:**
- `date: '%B %d, %Y'`

## Theme Architecture

### Directory Structure

```
theme/
├── assets/              # CSS, JS, images
├── config/              # Theme settings
│   ├── settings_schema.json
│   └── settings_data.json
├── layout/              # Base templates
│   └── theme.liquid
├── locales/             # Translations
│   └── en.default.json
├── sections/            # Reusable blocks
│   ├── header.liquid
│   ├── footer.liquid
│   └── product-grid.liquid
├── snippets/            # Small components
│   ├── product-card.liquid
│   └── icon.liquid
└── templates/           # Page templates
    ├── index.json
    ├── product.json
    ├── collection.json
    └── cart.liquid
```

### Layout

Base template wrapping all pages (`layout/theme.liquid`):

```liquid
<!DOCTYPE html>
<html lang="{{ request.locale.iso_code }}">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>{{ page_title }}</title>

  {{ content_for_header }}

  <link rel="stylesheet" href="{{ 'theme.css' | asset_url }}">
</head>
<body>
  {% section 'header' %}

  <main>
    {{ content_for_layout }}
  </main>

  {% section 'footer' %}

  <script src="{{ 'theme.js' | asset_url }}"></script>
</body>
</html>
```

### Templates

Page-specific structures (`templates/product.json`):

```json
{
  "sections": {
    "main": {
      "type": "product-template",
      "settings": {
        "show_vendor": true,
        "show_quantity_selector": true
      }
    },
    "recommendations": {
      "type": "product-recommendations"
    }
  },
  "order": ["main", "recommendations"]
}
```

Legacy format (`templates/product.liquid`):
```liquid
<div class="product">
  <div class="product-images">
    <img src="{{ product.featured_image | img_url: 'large' }}" alt="{{ product.title }}">
  </div>

  <div class="product-details">
    <h1>{{ product.title }}</h1>
    <p class="price">{{ product.price | money }}</p>

    {% form 'product', product %}
      <select name="id">
        {% for variant in product.variants %}
          <option value="{{ variant.id }}">{{ variant.title }} - {{ variant.price | money }}</option>
        {% endfor %}
      </select>

      <button type="submit">Add to Cart</button>
    {% endform %}
  </div>
</div>
```

### Sections

Reusable content blocks (`sections/product-grid.liquid`):

```liquid
<div class="product-grid">
  {% for product in section.settings.collection.products %}
    <div class="product-card">
      <a href="{{ product.url }}">
        <img src="{{ product.featured_image | img_url: 'medium' }}" alt="{{ product.title }}">
        <h3>{{ product.title }}</h3>
        <p>{{ product.price | money }}</p>
      </a>
    </div>
  {% endfor %}
</div>

{% schema %}
{
  "name": "Product Grid",
  "settings": [
    {
      "type": "collection",
      "id": "collection",
      "label": "Collection"
    },
    {
      "type": "range",
      "id": "products_per_row",
      "min": 2,
      "max": 5,
      "step": 1,
      "default": 4,
      "label": "Products per row"
    }
  ],
  "presets": [
    {
      "name": "Product Grid"
    }
  ]
}
{% endschema %}
```

### Snippets

Small reusable components (`snippets/product-card.liquid`):

```liquid
<div class="product-card">
  <a href="{{ product.url }}">
    {% if product.featured_image %}
      <img src="{{ product.featured_image | img_url: 'medium' }}" alt="{{ product.title }}">
    {% endif %}
    <h3>{{ product.title }}</h3>
    <p class="price">{{ product.price | money }}</p>
    {% if product.compare_at_price > product.price %}
      <p class="sale-price">{{ product.compare_at_price | money }}</p>
    {% endif %}
  </a>
</div>
```

Include snippet:
```liquid
{% render 'product-card', product: product %}
```

## Development Workflow

> ⚠️ **IMPORTANT pour ce projet** : Le thème `perolles29-shopify/main` est synchronisé avec GitHub.
> Ne JAMAIS utiliser `shopify theme push` ou `shopify theme pull` sur ce projet.
> Utiliser uniquement `git push origin main` pour déployer.

### Setup

```bash
# Initialize new theme (nouveaux projets seulement)
shopify theme init

# Choose Dawn (reference theme) or blank
```

### Local Development

```bash
# Start local server for preview
shopify theme dev --store perolles29.myshopify.com

# Preview at http://localhost:9292
# Changes are previewed locally but NOT deployed
```

### Déployer les changements (ce projet)

```bash
# Récupérer les derniers changements
git pull origin main

# Après modifications, committer et pusher
git add .
git commit -m "feat: description"
git push origin main

# Shopify se synchronise automatiquement avec GitHub
```

### Theme Check

Lint theme code:
```bash
shopify theme check
shopify theme check --auto-correct
```

## Common Patterns

### Product Form with Variants

```liquid
{% form 'product', product %}
  {% unless product.has_only_default_variant %}
    {% for option in product.options_with_values %}
      <div class="product-option">
        <label>{{ option.name }}</label>
        <select name="options[{{ option.name }}]">
          {% for value in option.values %}
            <option value="{{ value }}">{{ value }}</option>
          {% endfor %}
        </select>
      </div>
    {% endfor %}
  {% endunless %}

  <input type="hidden" name="id" value="{{ product.selected_or_first_available_variant.id }}">
  <input type="number" name="quantity" value="1" min="1">

  <button type="submit" {% unless product.available %}disabled{% endunless %}>
    {% if product.available %}Add to Cart{% else %}Sold Out{% endif %}
  </button>
{% endform %}
```

### Pagination

```liquid
{% paginate collection.products by 12 %}
  {% for product in collection.products %}
    {% render 'product-card', product: product %}
  {% endfor %}

  {% if paginate.pages > 1 %}
    <div class="pagination">
      {% if paginate.previous %}
        <a href="{{ paginate.previous.url }}">Previous</a>
      {% endif %}

      {% for part in paginate.parts %}
        {% if part.is_link %}
          <a href="{{ part.url }}">{{ part.title }}</a>
        {% else %}
          <span class="current">{{ part.title }}</span>
        {% endif %}
      {% endfor %}

      {% if paginate.next %}
        <a href="{{ paginate.next.url }}">Next</a>
      {% endif %}
    </div>
  {% endif %}
{% endpaginate %}
```

### Cart AJAX

```javascript
// Add to cart
fetch('/cart/add.js', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    id: variantId,
    quantity: 1
  })
})
.then(res => res.json())
.then(item => console.log('Added:', item));

// Get cart
fetch('/cart.js')
  .then(res => res.json())
  .then(cart => console.log('Cart:', cart));

// Update cart
fetch('/cart/change.js', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    id: lineItemKey,
    quantity: 2
  })
})
.then(res => res.json());
```

## Metafields in Themes

Access custom data:

```liquid
{{ product.metafields.custom.care_instructions }}
{{ product.metafields.custom.material.value }}

{% if product.metafields.custom.featured %}
  <span class="badge">Featured</span>
{% endif %}
```

## Best Practices

**Performance:**
- Optimize images (use appropriate sizes)
- Minimize Liquid logic complexity
- Use lazy loading for images
- Defer non-critical JavaScript

**Accessibility:**
- Use semantic HTML
- Include alt text for images
- Support keyboard navigation
- Ensure sufficient color contrast

**SEO:**
- Use descriptive page titles
- Include meta descriptions
- Structure content with headings
- Implement schema markup

**Code Quality:**
- Follow Shopify theme guidelines
- Use consistent naming conventions
- Comment complex logic
- Keep sections focused and reusable

## Resources

- Theme Development: https://shopify.dev/docs/themes
- Liquid Reference: https://shopify.dev/docs/api/liquid
- Dawn Theme: https://github.com/Shopify/dawn
- Theme Check: https://shopify.dev/docs/themes/tools/theme-check
