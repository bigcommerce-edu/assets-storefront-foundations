# Bundle and Upload a Theme

## Bundle Your Theme

**`config.json`:**

```json
{
  "name": "My Theme",
  "version": "1.0.0",
  ...
}
```

**Bundle:**

```shell
stencil bundle
```

## Upload and Apply Your Theme

**Push:**

```shell
stencil push
```

**Push with variation:**

```shell
stencil push --activate <variation_name>
```

**Push with channel IDs:**

```shell
stencil push --activate <variation_name> --channel_ids 123 456
```
