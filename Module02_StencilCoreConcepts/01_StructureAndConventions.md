# Stencil Structure and Conventions

## Theme Configuration

### Accessing Values in Templates

**Accessing in templates:**

```
{{theme_settings.brandpage_products_per_page}}
```

**Accessing in SCSS:**

```
$body-font-family:  stencilFontFamily("body-font"), Arial, Helvetica, sans-serif;
```

**Injecting into JavaScript:**

```
{{inject "brandProductsPerPage" theme_settings.brandpage_products_per_page}}
```

**Accessing in JavaScript:**

```javascript
const productsPerPage = this.context.brandProductsPerPage;
```
