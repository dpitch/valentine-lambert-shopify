# Stretch Theme — JavaScript Events & Web Components

> Applies to Stretch, Impact, and Prestige v7+ themes by Maestrooo.

## Custom JavaScript Events

The theme fires custom events on `document` that you can listen to from `custom.js`.

### Product Events

#### `variant:change`
Fired when a user selects a different variant.

```javascript
document.addEventListener('variant:change', function(event) {
  let variant = event.detail.variant;         // New variant object
  let previousVariant = event.detail.previousVariant; // Previous variant

  console.log('Selected variant:', variant.title, variant.price);
});
```

**Target**: The form controlling the variant
**Payload**: `{ variant, previousVariant }`

### Cart Events

#### `variant:add`
Triggered when variant(s) are added to cart via form selectors.

```javascript
document.addEventListener('variant:add', function(event) {
  let items = event.detail.items; // Array of added items
  let cart = event.detail.cart;   // Updated cart object
});
```

**Payload**: `{ items, cart }`

#### `line-item:change`
Emitted when a line item quantity changes.

```javascript
document.addEventListener('line-item:change', function(event) {
  let quantity = event.detail.quantity; // New quantity
  let cart = event.detail.cart;        // Updated cart object
});
```

**Payload**: `{ quantity, cart }`

#### `cart:change`
Fires whenever cart content changes (additions, removals, quantity updates).

```javascript
document.addEventListener('cart:change', function(event) {
  let cart = event.detail.cart;           // Updated cart
  let baseEvent = event.detail.baseEvent; // What triggered: 'variant:add', 'line-item:change', etc.
});
```

**Payload**: `{ cart, baseEvent }`

#### `cart:error`
Fired when adding products fails (e.g., out of stock).

```javascript
document.addEventListener('cart:error', function(event) {
  let error = event.detail.error; // Shopify error message
  console.error('Cart error:', error);
});
```

**Payload**: `{ error }`

#### `cart:refresh` (dispatchable)
Manually trigger a cart UI refresh:

```javascript
document.documentElement.dispatchEvent(new CustomEvent('cart:refresh', {
  bubbles: true
}));
```

### Dialog Events

#### `dialog:after-show`
Fired after a drawer or modal finishes opening.

#### `dialog:after-hide`
Fired after a drawer or modal finishes closing.

## Exported Utilities (from theme.js)

Import via ES modules in your `custom.js`:

```javascript
<script type="module">
  import { fetchCart, throttle, debounce } from "theme";
  import { animate, timeline, inView, scroll, ScrollOffset } from "vendor";
</script>
```

### `fetchCart()`
Returns a Promise with the current cart as JSON. Smart caching — multiple calls don't trigger redundant Ajax requests.

```javascript
const cart = await fetchCart();
console.log('Cart items:', cart.items.length);
```

### `createMediaImg(media, widths, attributes)`
Generates `<img>` elements from Shopify media objects. Handles srcset URL generation automatically.

### `imageLoaded(img)`
Returns a Promise that resolves when the image is fully loaded.

### `throttle(callback, delay)`
Limits callback execution frequency. Use for scroll/resize handlers.

```javascript
window.addEventListener('scroll', throttle(() => {
  console.log('Scrolled');
}, 100));
```

### `debounce(callback, delay = 150)`
Delays callback execution until activity stops. Use for search inputs.

```javascript
input.addEventListener('input', debounce((e) => {
  console.log('Search:', e.target.value);
}, 300));
```

## Web Components

### Drawers (`<x-drawer>`)
Off-screen panels using shadow DOM.

```html
<button aria-controls="my-drawer">Open Drawer</button>
<x-drawer id="my-drawer">
  <h2 slot="header">Drawer Title</h2>
  <p>Drawer content goes here</p>
</x-drawer>
```

**Slots**: `header` (title), default (body content)
**Methods**: `.show()` → Promise, `.hide()` → Promise
**Events**: `dialog:after-show`, `dialog:after-hide`

### Popovers (`<x-popover>`)
Contextual floating elements.

```html
<x-popover anchor-vertical="start" anchor-horizontal="center">
  <h2 slot="title">Popover Title</h2>
  <p>Popover content</p>
</x-popover>
```

**Attributes**:
- `anchor-vertical`: "start" | "center" | "end"
- `anchor-horizontal`: "start" | "center" | "end"

### Product Forms (`is="product-form"`)
Add to any `<form>` for automatic Ajax cart integration (no page reload).

```html
<form is="product-form" action="/cart/add" method="post">
  <!-- variant selector, quantity, submit button -->
</form>
```

## Extending Components

Create custom components by extending theme components:

```javascript
// custom.js
import { Drawer } from "theme";

class CustomDrawer extends Drawer {
  connectedCallback() {
    super.connectedCallback();
    // Custom initialization
  }
}

window.customElements.define('custom-drawer', CustomDrawer);
```

## Animation with Motion v12

Stretch uses Motion v12.4.7 (different API from v10 in other themes).

```javascript
import { animate, timeline, inView, scroll, ScrollOffset } from "vendor";

// Simple animation
animate(element, { opacity: [0, 1], y: [20, 0] }, { duration: 0.5 });

// Scroll-triggered
inView(element, () => {
  animate(element, { opacity: 1 }, { duration: 0.3 });
});

// Scroll-linked
scroll(animate(element, { x: [0, 100] }), {
  target: element,
  offset: [ScrollOffset.Enter, ScrollOffset.Exit]
});
```

## Best Practices

1. **Always use `custom.js`** — Never modify `theme.js` or `vendor.min.js`
2. **Use ES modules** — `<script type="module">` for proper scoping
3. **Listen to theme events** — Don't duplicate cart/variant logic
4. **Use theme utilities** — `fetchCart()`, `throttle()`, `debounce()` are optimized
5. **Extend, don't replace** — Subclass existing web components
6. **Keep bundles small** — Theme is already optimized; don't add heavy libraries
