# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Tropical Treats is a static e-commerce website for "Pine & Ginger Coconut Drops" - a Caribbean-inspired snack product created by a college student entrepreneur. The site is built with vanilla HTML, CSS, and JavaScript with no build process or external dependencies.

## Architecture

### Core Pattern: Self-Contained HTML Pages

Each HTML file is completely self-contained with:
- Inline CSS in `<style>` tags within `<head>`
- Inline JavaScript in `<script>` tags before closing `</body>`
- No external CSS or JS files
- No build system or bundler

### Shopping Cart Implementation

The shopping cart uses **localStorage** for client-side state management:
- Storage key: `'tropicalTreatsCart'`
- Cart data structure: Array of objects with `{ name, price, quantity }` or `{ id, name, price, displayPrice, quantity }`
- Cart functions are duplicated across multiple pages (not centralized)
- Cart count badge is updated via `updateCartDisplay()` function on page load

**Key Cart Functions** (repeated across pages):
- `getCart()` - Retrieves cart from localStorage
- `saveCart(cart)` - Saves cart to localStorage
- `updateCartDisplay()` - Updates cart icon counter in navigation
- `addToCart()` - Adds items to cart

### Design System

CSS custom properties (CSS variables) define the color scheme in `:root`:
```css
--color-green: #38761d    /* Primary green */
--color-brown: #5c3b1e    /* Secondary brown */
--color-gold: #ffd966     /* Accent gold/yellow */
--color-white: #ffffff
--color-light-gray: #f5f5f5
--color-text: #333333
```

Typography: Google Fonts "Poppins" (weights: 300, 400, 600, 700)

### Page Structure

**Navigation Pattern**: All pages share identical header with:
- Logo container with `logo-sm.png` image
- Navigation links: Home, Products, About, Investors, Resources
- Shopping cart icon with live item count

**Main Pages**:
- `index.html` - Homepage with two-column layout (sidebar with story/benefits, product display with package selection)
- `products.html` - Product grid showing all three package sizes with detailed descriptions
- `cart.html` - Shopping cart view with quantity controls and remove buttons
- `checkout.html` - Checkout form (simulated payment, not functional)
- `success.html` - Order confirmation page
- `about.html` - Founder's story with section navigation and reading progress bar
- `investors.html` - Information for potential investors
- `resources.html` - Additional resources

### Product Packages

Three package sizes available:
1. **Small Pack** - $150 JMD
2. **Medium Pack** - $250 JMD
3. **Large Pack** - $350 JMD

## Development Workflow

### No Build Process

This is a static site with no compilation or build steps:
- Edit HTML files directly
- Changes are immediately viewable in browser
- No `npm install`, `npm run build`, or similar commands needed

### Testing Locally

Simply open HTML files in a web browser:
```bash
# Open in default browser (Linux)
xdg-open index.html

# Or use a local web server for better localStorage behavior
python3 -m http.server 8000
# Then visit http://localhost:8000
```

### Git Workflow

Repository uses `main` as the primary branch. Modified files currently include:
- All main HTML pages (about.html, cart.html, checkout.html, index.html, success.html)
- Untracked: investors.html, products.html, resources.html, images

## Key Implementation Details

### Adding Products to Cart

Two different implementations exist:

**index.html** (complex):
- Uses dropdown `<select>` with pipe-delimited values: `"Name|DisplayPrice|NumericPrice"`
- Parses selection in `handleAddToCart()`
- Creates item object with `id` generated from name

**products.html** (simple):
- Direct button onclick: `addToCart('Small Pack', 150)`
- Simpler data structure without `displayPrice` or `id` fields

### Cart Page Functionality

- Displays items in table format
- Quantity controls: `<input type="number">` with `onchange="updateQuantity(index, value)"`
- Remove button with confirmation dialog
- Auto-hides checkout button when cart is empty
- Calculates subtotal in JMD currency format

### Checkout Flow

1. User adds items to cart (localStorage updated)
2. Views cart at `cart.html`
3. Proceeds to `checkout.html`
4. Submits form (validation only, no actual payment processing)
5. Form submission clears localStorage and redirects to `success.html`

### Responsive Design

Mobile breakpoints typically at `max-width: 768px` or `900px`:
- Navigation becomes vertical/wraps
- Two-column layouts stack to single column
- Font sizes reduce
- Padding/margins adjust

## Important Notes

### Consistency Considerations

When modifying functionality across pages:
- Cart functions are duplicated across multiple files
- Changes to cart logic must be replicated in: index.html, products.html, cart.html, checkout.html, about.html
- Header/navigation HTML is duplicated across all pages
- CSS variables ensure color consistency but styles are otherwise duplicated

### Currency

All prices displayed in **JMD (Jamaican Dollars)**, not USD

### Images

Logo image: `logo-sm.png` (used in header across all pages)
Product images: `product-hero.jpg`, `products.jpg`, `packages.jpg`

### Known Issues

- Line 189 in index.html has typo: `max-height: 108f0px;` (should be `1080px`)
- No centralized JavaScript - functions duplicated across files
- No form validation beyond HTML5 `required` attributes
- Checkout is simulation only with disabled card input field
