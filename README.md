<h1 align="center" style="position: relative;">
  <br>
    <img src="./assets/shoppy-x-ray.svg" alt="logo" width="200">
  <br>
  Shopify Skeleton Theme
</h1>

A minimal, carefully structured Shopify theme designed to help you quickly get started. Designed with modularity, maintainability, and Shopify's best practices in mind.

<p align="center">
  <a href="./LICENSE.md"><img src="https://img.shields.io/badge/License-MIT-green.svg" alt="License"></a>
  <a href="./actions/workflows/ci.yml"><img alt="CI" src="https://github.com/Shopify/skeleton-theme/actions/workflows/ci.yml/badge.svg"></a>
</p>

## Getting started

### Prerequisites

Before starting, ensure you have the latest Shopify CLI installed:

- [Shopify CLI](https://shopify.dev/docs/api/shopify-cli) – helps you download, upload, preview themes, and streamline your workflows

If you use VS Code:

- [Shopify Liquid VS Code Extension](https://shopify.dev/docs/storefronts/themes/tools/shopify-liquid-vscode) – provides syntax highlighting, linting, inline documentation, and auto-completion specifically designed for Liquid templates

### Clone

Clone this repository using Git or Shopify CLI:

```bash
git clone git@github.com:Shopify/skeleton-theme.git
# or
shopify theme init
```

### Preview

Preview this theme using Shopify CLI:

```bash
shopify theme dev
```

## Theme architecture

```bash
.
├── assets          # Stores static assets (CSS, JS, images, fonts, etc.)
├── blocks          # Reusable, nestable, customizable UI components
├── config          # Global theme settings and customization options
├── layout          # Top-level wrappers for pages (layout templates)
├── locales         # Translation files for theme internationalization
├── sections        # Modular full-width page components
├── snippets        # Reusable Liquid code or HTML fragments
└── templates       # Templates combining sections to define page structures
```

To learn more, refer to the [theme architecture documentation](https://shopify.dev/docs/storefronts/themes/architecture).

### Templates

[Templates](https://shopify.dev/docs/storefronts/themes/architecture/templates#template-types) control what's rendered on each type of page in a theme.

The Skeleton Theme scaffolds [JSON templates](https://shopify.dev/docs/storefronts/themes/architecture/templates/json-templates) to make it easy for merchants to customize their store.

None of the template types are required, and not all of them are included in the Skeleton Theme. Refer to the [template types reference](https://shopify.dev/docs/storefronts/themes/architecture/templates#template-types) for a full list.

### Sections

[Sections](https://shopify.dev/docs/storefronts/themes/architecture/sections) are Liquid files that allow you to create reusable modules of content that can be customized by merchants. They can also include blocks which allow merchants to add, remove, and reorder content within a section.

Sections are made customizable by including a `{% schema %}` in the body. For more information, refer to the [section schema documentation](https://shopify.dev/docs/storefronts/themes/architecture/sections/section-schema).

### Blocks

[Blocks](https://shopify.dev/docs/storefronts/themes/architecture/blocks) let developers create flexible layouts by breaking down sections into smaller, reusable pieces of Liquid. Each block has its own set of settings, and can be added, removed, and reordered within a section.

Blocks are made customizable by including a `{% schema %}` in the body. For more information, refer to the [block schema documentation](https://shopify.dev/docs/storefronts/themes/architecture/blocks/theme-blocks/schema).

## Schemas

When developing components defined by schema settings, we recommend these guidelines to simplify your code:

- **Single property settings**: For settings that correspond to a single CSS property, use CSS variables:

  ```liquid
  <div class="collection" style="--gap: {{ block.settings.gap }}px">
    ...
  </div>

  {% stylesheet %}
    .collection {
      gap: var(--gap);
    }
  {% endstylesheet %}

  {% schema %}
  {
    "settings": [{
      "type": "range",
      "label": "gap",
      "id": "gap",
      "min": 0,
      "max": 100,
      "unit": "px",
      "default": 0,
    }]
  }
  {% endschema %}
  ```

- **Multiple property settings**: For settings that control multiple CSS properties, use CSS classes:

  ```liquid
  <div class="collection {{ block.settings.layout }}">
    ...
  </div>

  {% stylesheet %}
    .collection--full-width {
      /* multiple styles */
    }
    .collection--narrow {
      /* multiple styles */
    }
  {% endstylesheet %}

  {% schema %}
  {
    "settings": [{
      "type": "select",
      "id": "layout",
      "label": "layout",
      "values": [
        { "value": "collection--full-width", "label": "t:options.full" },
        { "value": "collection--narrow", "label": "t:options.narrow" }
      ]
    }]
  }
  {% endschema %}
  ```

## CSS & JavaScript

For CSS and JavaScript, we recommend using the [`{% stylesheet %}`](https://shopify.dev/docs/api/liquid/tags#stylesheet) and [`{% javascript %}`](https://shopify.dev/docs/api/liquid/tags/javascript) tags. They can be included multiple times, but the code will only appear once.

### `critical.css`

The Skeleton Theme explicitly separates essential CSS necessary for every page into a dedicated `critical.css` file.

## Contributing

We're excited for your contributions to the Skeleton Theme! This repository aims to remain as lean, lightweight, and fundamental as possible, and we kindly ask your contributions to align with this intention.

Visit our [CONTRIBUTING.md](./CONTRIBUTING.md) for a detailed overview of our process, guidelines, and recommendations.

## License

Skeleton Theme is open-sourced under the [MIT](./LICENSE.md) License.
<!-- {% form 'product', product %}
  <label for="product-{{ product.id }}-variant">
    {{ 'products.product.choose_option' | t }}
  </label>
  <select
    id="product-{{ product.id }}-variant"
    name="id"
  >
    {% for variant in product.variants %}
      <option
        value="{{ variant.id }}"
        {% if variant == product.selected_or_first_available_variant %}
          selected
        {% endif %}
        {% unless variant.available %}disabled{% endunless %}
      >
        {{ variant.title }} – {{ variant.price | money }}
      </option>
    {% endfor %}
  </select>

  <button type="submit">
    {{ 'products.product.add_to_cart' | t }}
  </button>
{% endform %} -->

<section class="cart" aria-labelledby="cart-heading">
  <h1 id="cart-heading">{{ 'sections.cart.title' | t }}</h1>

  {% if cart == empty %}
    <p>{{ 'sections.cart.empty' | t }}</p>
    <a href="{{ routes.all_products_collection_url }}">{{ 'sections.cart.continue_shopping' | t }}</a>
  {% else %}
    {% form 'cart', cart, id: 'cart', class: 'cart__form' %}
      <table class="cart__table" role="table">
        <thead>
          <tr>
            <th scope="col">{{ 'sections.cart.column_product' | t }}</th>
            <th scope="col">{{ 'sections.cart.column_price' | t }}</th>
            <th scope="col">{{ 'sections.cart.column_quantity' | t }}</th>
            <th scope="col">{{ 'sections.cart.column_total' | t }}</th>
            <th scope="col">{{ 'sections.cart.column_remove' | t }}</th>
          </tr>
        </thead>
        <tbody>
          {% for item in cart.items %}
            <tr class="cart-item">
              <td class="cart-item__product">
                <a href="{{ item.url }}" class="cart-item__image">
                  {{ item.image | image_url: width: 180 | image_tag: loading: 'lazy' }}
                </a>
                <div class="cart-item__details">
                  <a href="{{ item.url }}" class="cart-item__title">{{ item.product.title | escape }}</a>
                  {% if item.variant.title != 'Default Title' %}
                    <p class="cart-item__variant">{{ item.variant.title | escape }}</p>
                  {% endif %}

                  {% unless item.properties == empty %}
                    <dl class="cart-item__properties">
                      {% for property in item.properties %}
                        <div class="cart-item__property">
                          <dt class="cart-item__property-label">{{ property.first | escape }}</dt>
                          <dd class="cart-item__property-value">
                            {% if property.last contains '/uploads/' %}
                              <a href="{{ property.last }}">
                                {{ property.last | split: '/' | last | escape }}
                              </a>
                            {% else %}
                              {{ property.last }}
                            {% endif %}
                          </dd>
                        </div>
                      {% endfor %}
                    </dl>
                  {% endunless %}
                </div>
              </td>

              <td class="cart-item__price" data-label="{{ 'sections.cart.column_price' | t }}">
                {{ item.final_price | money }}
                {% if item.original_price > item.final_price %}
                  <div class="cart-item__price-original">{{ item.original_price | money }}</div>
                {% endif %}
              </td>

              <td class="cart-item__quantity" data-label="{{ 'sections.cart.column_quantity' | t }}">
                <label class="visually-hidden" for="quantity-{{ item.key }}">{{ 'sections.cart.quantity_label' | t }}</label>
                <input
                  id="quantity-{{ item.key }}"
                  class="cart-item__quantity-input"
                  type="number"
                  name="updates[]"
                  min="0"
                  value="{{ item.quantity }}"
                  inputmode="numeric"
                />
              </td>

              <td class="cart-item__total" data-label="{{ 'sections.cart.column_total' | t }}">
                {{ item.final_line_price | money }}
              </td>

              <td class="cart-item__remove">
                <a href="{{ item.url_to_remove }}" class="cart-item__remove-link">
                  {{ 'sections.cart.remove' | t }}
                </a>
              </td>
            </tr>
          {% endfor %}
        </tbody>
      </table>

      <footer class="cart__footer">
        <div class="cart__notes">
          <label for="cart-note">{{ 'sections.cart.notes_label' | t }}</label>
          <textarea
            id="cart-note"
            name="note"
            rows="3"
          >{{ cart.note }}</textarea>
        </div>

        <aside class="cart__summary" aria-label="{{ 'sections.cart.summary_aria_label' | t }}">
          <dl class="cart__totals">
            <div class="cart__totals-row">
              <dt>{{ 'sections.cart.subtotal' | t }}</dt>
              <dd>{{ cart.subtotal_price | money }}</dd>
            </div>

            {% if cart.total_discount > 0 %}
              <div class="cart__totals-row cart__totals-row--discounts">
                <dt>{{ 'sections.cart.discounts' | t }}</dt>
                <dd>-{{ cart.total_discount | money }}</dd>
              </div>
            {% endif %}

            <p class="cart__tax-note">{{ 'sections.cart.taxes_and_shipping_at_checkout' | t }}</p>
          </dl>

          <button type="submit" name="update" class="cart__update">
            {{ 'sections.cart.update' | t }}
          </button>

          <button type="submit" name="checkout" class="cart__checkout">
            {{ 'sections.cart.checkout' | t }}
          </button>
        </aside>
      </footer>
    {% endform %}
  {% endif %}
</section>

{% stylesheet %}
  .cart {
    max-width: 72rem;
    margin: 0 auto;
    padding: 2rem 1.5rem 3rem;
  }

  .cart__table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 1.5rem;
  }

  .cart__table th,
  .cart__table td {
    padding: 0.75rem 0.5rem;
    vertical-align: top;
  }

  .cart-item__product {
    display: grid;
    grid-template-columns: auto 1fr;
    gap: 0.75rem;
    align-items: flex-start;
  }

  .cart-item__image img {
    border-radius: 0.5rem;
    display: block;
  }

  .cart-item__title {
    font-weight: 600;
    text-decoration: none;
  }

  .cart-item__title:hover,
  .cart-item__title:focus-visible {
    text-decoration: underline;
  }

  .cart-item__variant {
    margin-top: 0.25rem;
    font-size: 0.875rem;
    color: rgba(0, 0, 0, 0.7);
  }

  .cart-item__properties {
    margin-top: 0.5rem;
    font-size: 0.875rem;
  }

  .cart-item__property {
    display: flex;
    gap: 0.25rem;
  }

  .cart-item__property-label {
    font-weight: 500;
  }

  .cart-item__price-original {
    font-size: 0.875rem;
    text-decoration: line-through;
    color: rgba(0, 0, 0, 0.6);
  }

  .cart-item__quantity-input {
    width: 4rem;
  }

  .cart__footer {
    display: grid;
    grid-template-columns: minmax(0, 2fr) minmax(0, 1fr);
    gap: 2rem;
    margin-top: 2rem;
    align-items: flex-start;
  }

  .cart__notes textarea {
    width: 100%;
    resize: vertical;
  }

  .cart__summary {
    border-radius: 0.75rem;
    padding: 1.5rem;
    border: 1px solid rgba(0, 0, 0, 0.08);
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }

  .cart__totals-row {
    display: flex;
    justify-content: space-between;
    margin-bottom: 0.5rem;
  }

  .cart__tax-note {
    margin-top: 0.5rem;
    font-size: 0.875rem;
    color: rgba(0, 0, 0, 0.7);
  }

  .cart__update,
  .cart__checkout {
    width: 100%;
    padding: 0.75rem 1rem;
    border-radius: 999px;
    border: none;
    cursor: pointer;
    font-weight: 600;
  }

  .cart__update {
    background: transparent;
    border: 1px solid rgba(0, 0, 0, 0.16);
  }

  .cart__checkout {
    background: black;
    color: white;
  }

  @media (max-width: 768px) {
    .cart__table thead {
      display: none;
    }

    .cart__table tbody tr {
      display: grid;
      grid-template-columns: 1fr 1fr;
      grid-template-areas:
        'product product'
        'price quantity'
        'total remove';
      border-top: 1px solid rgba(0, 0, 0, 0.06);
      padding-top: 1rem;
    }

    .cart-item__product {
      grid-area: product;
    }

    .cart-item__price {
      grid-area: price;
    }

    .cart-item__quantity {
      grid-area: quantity;
    }

    .cart-item__total {
      grid-area: total;
    }

    .cart-item__remove {
      grid-area: remove;
      text-align: right;
    }

    .cart__footer {
      grid-template-columns: 1fr;
    }
  }

  .visually-hidden {
    position: absolute;
    width: 1px;
    height: 1px;
    padding: 0;
    margin: -1px;
    overflow: hidden;
    clip: rect(0, 0, 0, 0);
    white-space: nowrap;
    border: 0;
  }
{% endstylesheet %}

{% schema %}
{
  "name": "t:sections.cart.name",
  "settings": [],
  "blocks": [],
  "presets": [
    {
      "name": "t:sections.cart.name"
    }
  ]
}
{% endschema %}