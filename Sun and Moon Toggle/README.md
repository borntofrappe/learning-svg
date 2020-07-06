Here I try to recreate the interesting visual and interaction showcased on [joshwcomeau.com](https://joshwcomeau.com/), to toggle between two different color palette.

The palette only changes the color and background property, but the demo is oriented more toward the necessary SVG syntax and the peculiar CSS timing functions.

## Syntax

In `moon.svg` and `sun.svg` you find the visuals for the sun and moon. In the `.html`, the two are merged into a single `<svg>` element, and its features are modified to show either version through `transform` properties.

Consider the following changes:

- the mask element adds two group elements to have the shadow created for the moon transitioned in its scale

  ```html
  <g transform="translate(20 -20)">
    <g id="shadow" transform="scale(0)">
      <use transform="scale(4)" href="#circle" fill="hsl(0, 0%, 0%)" />
    </g>
  </g>
  ```

  The first group is necessary to have the change in scale occur from the (20, -20) point of origin.

- the circle showing the celestial body is that for the moon

  ```html
  <g mask="url(#mask--circle)">
    <use transform="scale(4)" href="#circle" />
  </g>
  ```

  Since the `#shadow` element has a scale of `0`, no shadow is included, but the body is still scaled to consider the larger size of the moon. To this end, include a additional group to scale back to the size of the circle used for the sun.

  ```html
  <g transform="scale(0.75)">
    <g mask="url(#mask--circle)">
      <use transform="scale(4)" href="#circle" />
    </g>
  </g>
  ```

  I've added an additional value to the `transform` property, in the form of a rotation: `transform="scale(0.75) rotate(90)"`. The idea is to have CSS set the `transform` property to `scale(1)`, and have the rotation automatically transitioned to its default `0`.

- each dot around the sun is wrapped in a group element to have it change in size

```html
<g>
  <use transform="rotate(0) translate(0 38)" href="#circle" />
</g>
```

The wrapper ensures that the change in scale happens from the point `38` up the actual position of the circle. This gives the illustion that the element is translating and changing in size, while in fact it does only the latter.
