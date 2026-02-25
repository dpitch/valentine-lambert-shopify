# Horizon Theme Accessibility Standards

WCAG 2.2 AA compliance patterns used in the Shopify Horizon theme.

## Table of Contents

1. [Global Standards](#global-standards)
2. [Focus Management](#focus-management)
3. [Headings](#headings)
4. [Modals & Dialogs](#modals--dialogs)
5. [Carousels & Slideshows](#carousels--slideshows)
6. [Forms](#forms)
7. [Color & Contrast](#color--contrast)
8. [Images](#images)
9. [Navigation](#navigation)
10. [Product Cards](#product-cards)

---

## Global Standards

- Target WCAG 2.2 Level AA compliance
- Use semantic HTML elements (`<nav>`, `<main>`, `<article>`, `<section>`, `<aside>`, `<header>`, `<footer>`)
- Include skip-to-content link (rendered by `snippets/skip-to-content-link.liquid`)
- All interactive elements must be keyboard accessible
- Minimum touch target size: 44x44px
- Never use `tabindex` > 0
- Use `aria-live` regions for dynamic content updates
- Respect `prefers-reduced-motion` media query for animations

---

## Focus Management

### Key Rules

- Visible focus indicators on all interactive elements
- Focus styles must have minimum 3:1 contrast ratio
- Use `:focus-visible` (not `:focus`) for keyboard-only indicators
- Never remove focus outlines without replacement
- Manage focus when opening/closing modals, drawers, and dialogs

### Patterns

```css
/* Focus visible styles */
:focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
}

/* Remove default only when providing custom */
:focus:not(:focus-visible) {
  outline: none;
}
```

---

## Headings

### Key Rules

- Maintain logical heading hierarchy (h1 → h2 → h3, no skipping)
- One `<h1>` per page
- Use headings for structure, not styling (use CSS classes for visual size)
- In the Horizon theme, heading levels often come from settings

### Pattern

```liquid
<{{ block.settings.heading_tag | default: 'h2' }}
  class="heading {{ block.settings.heading_size }}"
>
  {{ block.settings.heading }}
</{{ block.settings.heading_tag | default: 'h2' }}>
```

---

## Modals & Dialogs

### Key Rules

- Use `<dialog>` element or `role="dialog"` with `aria-modal="true"`
- Include `aria-label` or `aria-labelledby`
- Trap focus inside the modal when open
- Return focus to trigger element on close
- Close on Escape key
- Include visible close button

### Pattern

```html
<dialog
  aria-label="{{ 'accessibility.modal_title' | t }}"
  aria-modal="true"
>
  <button
    type="button"
    aria-label="{{ 'accessibility.close' | t }}"
    class="dialog__close"
  >
    {% render 'icon', icon: 'close' %}
  </button>
  <!-- Content -->
</dialog>
```

---

## Carousels & Slideshows

### Key Rules

- Use `role="region"` with `aria-roledescription="carousel"` on container
- Each slide: `role="group"` with `aria-roledescription="slide"` and `aria-label="Slide X of Y"`
- Include previous/next buttons with `aria-label`
- Pause auto-play on hover/focus
- Respect `prefers-reduced-motion`
- Include slide indicators with current state

### Pattern

```html
<div
  role="region"
  aria-roledescription="carousel"
  aria-label="{{ section.settings.heading }}"
>
  <div class="slideshow__slides">
    <div
      role="group"
      aria-roledescription="slide"
      aria-label="{{ 'accessibility.slide_x_of_y' | t: current: 1, total: total }}"
    >
      <!-- Slide content -->
    </div>
  </div>
  <button aria-label="{{ 'accessibility.previous_slide' | t }}">
    {% render 'icon', icon: 'chevron-left' %}
  </button>
  <button aria-label="{{ 'accessibility.next_slide' | t }}">
    {% render 'icon', icon: 'chevron-right' %}
  </button>
</div>
```

---

## Forms

### Key Rules

- Every input must have an associated `<label>` (visible or `aria-label`)
- Use `aria-describedby` for help text and error messages
- Mark required fields with `required` attribute and visual indicator
- Error messages linked to inputs via `aria-describedby`
- Group related fields with `<fieldset>` and `<legend>`
- Submit buttons use `<button type="submit">`

### Pattern

```html
<form action="/contact" method="post">
  <div class="form-field">
    <label for="email">{{ 'forms.email' | t }}</label>
    <input
      type="email"
      id="email"
      name="contact[email]"
      required
      aria-describedby="email-error"
    >
    <span id="email-error" class="form-field__error" role="alert" hidden>
      {{ 'forms.email_error' | t }}
    </span>
  </div>
  <button type="submit">{{ 'forms.submit' | t }}</button>
</form>
```

---

## Color & Contrast

### Key Rules

- Text contrast minimum 4.5:1 (normal text) / 3:1 (large text)
- UI component contrast minimum 3:1
- Never use color alone to convey information
- Focus indicators minimum 3:1 contrast
- The Horizon color scheme system includes adaptive opacity variables that adjust based on background brightness

---

## Images

### Key Rules

- All `<img>` must have `alt` attribute
- Decorative images: `alt=""`
- Informative images: descriptive `alt` text
- Use `loading="lazy"` for below-fold images
- Use `{{ image | image_tag }}` which generates srcset automatically

---

## Navigation

### Key Rules

- Main navigation in `<nav>` with `aria-label`
- Current page indicated with `aria-current="page"`
- Dropdown menus keyboard accessible (Enter/Space to open, Escape to close)
- Mobile menu as dialog with focus trap
- Mega menus support arrow key navigation

---

## Product Cards

### Key Rules

- Card must be a single interactive element (link wrapping content)
- Price changes announced via `aria-live="polite"` region
- Sale prices use `<s>` for original price with screen reader context
- Variant swatches labelled with color/option name
- Quick-add button has descriptive `aria-label` including product name

### Pattern

```html
<div class="product-card">
  <a href="{{ product.url }}" aria-label="{{ product.title }}">
    {{ product.featured_image | image_url: width: 300 | image_tag: alt: product.title, loading: 'lazy' }}
    <h3>{{ product.title }}</h3>
    <div class="price" aria-live="polite">
      {% render 'price', product: product %}
    </div>
  </a>
</div>
```
