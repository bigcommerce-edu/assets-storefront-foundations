# Add a Subcategory Listing in Stencil

## Setup

**Start:**

```shell
stencil start
```

## Add the Subcategory List Logic

**Category template in `templates/pages`:**

```html
<div class="page">

    <!-- START NEW CODE -->
    {{#if category.subcategories}}
    
        <p>Subcategory listing placeholder</p>

    {{else}}
    <!-- END NEW CODE -->

      {{#if category.faceted_search_enabled}}
          <aside ...>
              ...
          </aside>
      <!--- REMOVE "else if category.subcategories" BLOCK -->
      {{else if theme_settings.shop_by_price_visibility}}
          <aside ...>
              ...
          </aside>
      {{/if}}

      <main ...>
          {{> components/category/product-listing}}
      </main>

    <!-- START NEW CODE -->
    {{/if}}
    <!-- END NEW CODE -->

    ...
</div>
```

## Add the Subcategory List Component

**`components/category/subcategory-listing.html`:**

```html
<ul class="subcategoryGrid">
  {{#each subcategories}}
  <li>
    <a href="{{url}}" class="subcategoryGrid-card">
      <h2>{{name}}</h2>
      <span>
        Shop {{product_count}} 
        {{#if product_count '>' 1}}products{{else}}product{{/if}}
      </span>
    </a>
  </li>
  {{/each}}
</ul>
```

**Category template in `templates/pages`:**

```html
{{#if category.subcategories}}
    
    <!-- START MODIFIED CODE -->
    {{> components/category/subcategory-listing subcategories=category.subcategories}}
    <!-- END MODIFIED CODE -->

{{else}}
```

## Add SCSS Styles

**`assets/scss/components/custom/subcategoryGrid/_subcategoryGrid.scss`:**

```scss
.subcategoryGrid {

  @include u-listBullets("none");
  @include grid-row();
  font-size: 0; // 1
  margin-bottom: spacing("single");

  li {
      @include grid-column(12, $float: none);
      display: inline-block;
      font-size: fontSize("base"); // 1
      vertical-align: top;
      margin-bottom: spacing("single");

      @include breakpoint("small") {
          width: grid-calc(1, 2);
      }

      @include breakpoint("medium") {
          width: grid-calc(1, 3);
      }
  }
}

.subcategoryGrid-card {
  display: block;
  border: 1px solid black;
  background-color: gray;
  text-align: center;
  padding: spacing("double");
  text-decoration: none;
  border-radius: $global-radius;

  h2 {
    margin: 0;
  }
}
```

**`assets/scss/components`:**

```scss
// Subcategory Grid
@import "custom/subcategoryGrid/subcategoryGrid";
```

## Utilize Theme Settings in SCSS

**`config.json`:**

```json
{
  "name": "Cornerstone",
  ...
  "settings": {
    "color-subcat-bg": "#f3f3f3",
    "color-subcat-border": "#555555",
    ...
  },
  ...
}
```

**`assets/scss/components/custom/subcategoryGrid/_subcategoryGrid.scss`:**

```scss
.subcategoryGrid-card {
  ...
  /* START MODIFIED CODE */
  border: 1px solid stencilColor("color-subcat-border");
  background-color: stencilColor("color-subcat-bg");
  /* END MODIFIED CODE */
  ...
}
```
