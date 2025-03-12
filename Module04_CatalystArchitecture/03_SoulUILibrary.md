# The Soul UI Library

## Styling Components

**Simple Tailwind example:**

```html
<div
  className={clsx(
    'pl-5 pt-5 text-4xl font-bold opacity-25 @xs:text-7xl'
  )}
>
  {title}
</div>
```

**Overriding component styles:**

```html
<div
  style={{
    '--card-light-text': '#777777',
  } as CSSProperties}
>
  <Card
    href="..."
    title="..."
  />
</div>
```
