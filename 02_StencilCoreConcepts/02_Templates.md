# Templates

## Blocks, Pages, and Layouts

**Example Layout Component:**

```html
<!DOCTYPE html>
<html>
    <head>
        <title>{{ head.title }}</title>
        {{#block "head"}} {{/block}}
    </head>
    <body>
        {{#block "page"}} {{/block}}
    </body>
</html>
```

**Example Page Component:**

```html
{{#partial "page"}}

{{> components/common/breadcrumbs ...}}

<main class="page">
    ...
</main>

{{/partial}}

{{> layout/base}}
```

## Handlebars

**Output value:**

```
{{product.brand.name}}
```

**Conditional logic:**

```
{{#if product.brand}}
    <h2 class="productView-brand">
        ...
    </h2>
{{/if}}
```

**Looping expression:**

```
{{#each products}}
    <li class="product">
        ...
    </li>
{{/each}}
```

## Stencil Objects

**Accessing product object:**

```
<span>{{product.sku}}</span>
```

**Debugging:**

```
http://localhost:3000/?debug=context
```

## Front Matter

**Example:**

```
---
products:
    new:
        limit: {{theme_settings.homepage_new_products_count}}
    featured:
        limit: {{theme_settings.homepage_featured_products_count}}
    top_sellers:
        limit: {{theme_settings.homepage_top_products_count}}
carousel: {{theme_settings.homepage_show_carousel}}
blog:
    recent_posts:
        limit: {{theme_settings.homepage_blog_posts_count}}
---
```

## Regions

**Example:**

```
{{{region name="header_bottom"}}}
```
