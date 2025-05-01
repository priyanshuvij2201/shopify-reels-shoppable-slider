# Customizing Shopify Reels Shoppable Slider

Everything lives in the **`<style>`** block at the top of `sections/reels-slider.liquid`, so you never need a build step.

---

## 1 Brand look & feel

| Element                       | Where to edit                                           | Example               |
| ----------------------------- | ------------------------------------------------------- | --------------------- |
| **Text / icon colour**        | `color:#7B2B26` (inside `.rc-info`, `.reels-card`)      | `color:#E91E63`       |
| **Corner radius**             | `border-radius:20px;` (in `.carousel-cell`)            | `border-radius:12px;` |
| **Gap between slides**        | `--reels-gap-m` (mobile) & `--reels-gap-d` (desktop)   | `--reels-gap-m:8px;`  |
| **Card hover tint (desktop)** | `.reels-card:hover { background: … }`                  | change alpha / hex    |
| **Thumbnail size**            | `.rc-image { width:48px; height:48px; }`               | `width:56px;`         |

---

## 2 Behaviour toggles

### 2.1 Add-to-Cart vs. Product-page link

**Default:** the card opens the product page.

To switch to instant add-to-cart:

```liquid
<!-- Replace the anchor wrapper -->
<button class="reels-card" data-variant-id="{{ variant.id }}">

### 2.1 Ensure the Add-to-Cart listener exists

```js
document.addEventListener('click', async (e) => {
  const btn = e.target.closest('[data-variant-id]');
  if (!btn) return;
  await fetch('/cart/add.js', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ items: [{ id: btn.dataset.variantId, quantity: 1 }] })
  });
  window.location = '/cart';      // or open cart drawer
});

### 2.2 Video autoplay speed

Find this attribute inside the section markup:

```liquid
data-flickity='{"autoPlay":5500,…}'

Change 5500 (milliseconds) to your desired autoplay interval.

### Multiple products per Reel

Add extra product settings to your schema:

```liquid
{ "type": "product", "id": "product_2", "label": "Related product 2" }
{ "type": "product", "id": "product_3", "label": "Related product 3" }'

Gather them in Liquid:


```liquid
{% assign products = '' | split: ',' %}
{% for key in (1..3) %}
  {% capture k %}product_{{ key }}{% endcapture %}
  {% if block.settings[k] %}
    {% assign products = products | push: block.settings[k] %}
  {% endif %}
{% endfor %}'

Then loop through the products array to render extra cards, buttons, or images.

## 3 Advanced tweaks

| Feature                         | Snippet                                                                                                   |
| ------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| **Retina poster**               | Use `poster | image_url: width:800` instead of `width:400` to improve image quality on high-DPI screens.     |
| **Analytics hook**              | Inside your click listener: `gtag('event', 'reel_click', { product_id: variant.id });`                    |
| **Disable autoplay on desktop** | In Flickity JSON: `"autoPlay": window.matchMedia('(hover: none)').matches ? 5500 : false`                 |









