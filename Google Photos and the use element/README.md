Project introducing the `<use>` element.

You can repeat the same shape:

```html
<path fill="hsl(5, 81%, 56%)" d="M 0 0 v -50 a 25 25 0 0 1 0 50" />
<path
  fill="hsl(217, 89%, 61%)"
  transform="rotate(90)"
  d="M 0 0 v -50 a 25 25 0 0 1 0 50"
/>
<path
  fill="hsl(136, 53%, 43%)"
  transform="rotate(180)"
  d="M 0 0 v -50 a 25 25 0 0 1 0 50"
/>
<path
  fill="hsl(45, 97%, 50%)"
  transform="rotate(270)"
  d="M 0 0 v -50 a 25 25 0 0 1 0 50"
/>
```

Alternatively, you can **define** the shape, and later **re-use** it with the `<use>` element.

```html
<defs>
  <path id="leaf" d="M 0 0 v -50 a 25 25 0 0 1 0 50" />
</defs>

<use href="#leaf" fill="hsl(5, 81%, 56%)" />
<use href="#leaf" fill="hsl(217, 89%, 61%)" transform="rotate(90)" />
<use href="#leaf" fill="hsl(136, 53%, 43%)" transform="rotate(180)" />
<use href="#leaf" fill="hsl(45, 97%, 50%)" transform="rotate(270)" />
```

## Advantages

By repeating the same shape, it means you only need to change the syntax in the `defs` block to have it modify every instance. In this light, it functions as similarly to a component in a design system.

In terms of size, the benefits depend on the complexity of the element being defined. In the project at hand, it is actually more efficient to repeat multiple `<path>` elements. See the size for `logo-use` and `logo-paths` for a comparison. Each graphic has been processed through [SVGOMG](https://jakearchibald.github.io/svgomg/) so that the syntax has been optimized.

## Pay attention

The `<use>` element does not accept every attribute. You can set [\_presentational attributes](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/Presentation) like `fill` and `stroke`, but not attributes characteristics of the defined element, like `d` for the `<path>` element.

```html
<!-- works -->
<use href="#leaf" fill="purple" />

<!-- does NOT work 
does not change the `d` attribute set on the defined shape
-->
<use href="#leaf" d="M 0 0 l 50 50" />
```

When setting a presentational attribute, also remember that these **do not** override the attributes set on the defined shape. If you were to define the leaf with a `fill` attribute.

```html
<defs>
  <path id="leaf" fill="hsl(5, 81%, 56%)" d="M 0 0 v -50 a 25 25 0 0 1 0 50" />
</defs>
```

Every `<use>` element will use the given color. Even if you specify a different value.

```html
<use href="#leaf" fill="hsl(217, 89%, 61%)" />
```

The color of the shape triumphs that which is set on the `<use>` element.
