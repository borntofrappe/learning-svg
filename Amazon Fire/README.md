This project serves to illustrate a few SVG concepts:

- it's possible to use a `<text>` element for a mask

- a linear gradient is set up with a `<linearGradient>` element; it is defined in the `<defs>` block and later referred to with the `url(#identifier)` syntax

- it's possible to use gradients, mask, anything defined in the `<defs>` block _in other elements_ of the `<defs>` block

## Masked text

To build the demo, in increments and starting with the static text:

- draw a rectangle

  ```html
  <rect width="100" height="100" fill="currentColor" />
  ```

- define a gradient

  ```html
  <linearGradient id="gradient-fire" x1="1" x2="0" y1="0" y2="1">
    <stop offset="0" stop-color="red" />
    <stop offset="1" stop-color="orange" />
  </linearGradient>
  ```

- apply the gradient to the rectangle

  ```html
  <rect width="100" height="100" fill="url(#gradient-fire)" />
  ```

- define the mask

  ```html
  <mask id="mask-fire">
    <g fill="white">
      <text>
        fire
      </text>
    </g>
  </mask>
  ```

  There are additional properties to change the styling of the text, but what matters is the `fill`. Set to white, it allows to show the shape described by the `<text>` element

- apply the mask to the the rectangle

  ```html
  <rect
    width="100"
    height="100"
    fill="url(#gradient-fire)"
    mask="url(#mask-fire)"
  />
  ```

  Ultimately, I decided to apply the mask on a wrapping `<group>` element, but the point stands.

## Masked gradient

For the second layer of the demo, the idea is to have a brighter gradient on top of the masked rectangle. Using the same mask (the one described by the text element), and by translating the shape left to right, the "glow" is applied to provide a pleasing effect.

The process is rather similar to the masked text, but has an additional step to guarantee that the gradient doesn't begin/end with a hard stop.

- define a rectangle

```html
<rect width="100" height="100" fill="currentColor" />
```

- define the brighter gradient

```html
<linearGradient id="gradient-light" x1="0" x2="1" y1="0" y2="1">
  <stop offset="0" stop-color="orange" />
  <stop offset="0.5" stop-color="yellow" />
  <stop offset="1" stop-color="orange" />
</linearGradient>
```

- apply the gradient

```html
<rect width="100" height="100" fill="url(#gradient-light)" />
```

This creates a gradient already. However, the gradient start and end with a hard stop. To see this, move the rectangle to either side

```html
<rect x="30" width="100" height="100" fill="url(#gradient-light)" />
```

And notice how the shape begins abruptly.

To have the gradient begin and end more softly, the idea is to render the edges of the rectangle progressively transparent. This by using another mask, and leveraging the fact that the mask progressively hides the elements the closer you get to a color of `hsl(0, 0%, 0%)`. It is perhaps better explained in practice:

- define a gradient that goes from black to white and back to black

```html
<linearGradient id="gradient-mask" x1="0" x2="1" y1="0.5" y2="0.5">
  <stop offset="0" stop-color="black" />
  <stop offset="0.5" stop-color="white" />
  <stop offset="1" stop-color="black" />
</linearGradient>
```

Notice how it goes from left to write.

- define a mask which uses the gradient with a rectangle of the same size as the one drawn in the body of the SVG element

```html
<mask id="mask-light">
  <rect width="100" height="100" fill="url(#gradient-mask)" />
</mask>
```

- apply the mask to the rectangle

```html
<rect
  x="30"
  width="100"
  height="100"
  fill="url(#gradient-light)"
  mask="url(#mask-ight)"
/>
```

This guarantees the brighter gradient is completely transparent at either end, and becomes fully opaque in its center.

## Translation

The masked, and brighter-gradient rectangle, is translated horizontally to go from side to side. I've noticed that firefox seems to have issues when this translation mingles SVG attributes and CSS properties.

```html
<g id="translate" transform="translate(-100 0)"> </g>
```

```css
#translate {
  animation: translate 2s;
}
@keyframes translate {
  to {
    transform: translateX(100px);
  }
}
```

In light of this, I've decided to update the SVG syntax to use the `transform` property instead of the presentational attribute

```html
<g id="translate" style="transform: translateX(-200px)"> </g>
```
