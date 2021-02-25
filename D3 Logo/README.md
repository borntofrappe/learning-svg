# D3 Logo

[D3.js community](https://d3js.community/hello-world) displays the logo for the D3 library with a series of tracing lines. Here I try to recreate the visual with SVG syntax.

## Tracing

`tracing.svg` draws the guiding lines for the D3 logo,

## Masking

`d3.svg` adds a series of `<mask>` elements to progressively show and hide portions of the visual.

Finally, the graphic removes the tracing lines and manipulates the `viewBox` attribute to remove the space around the logo. The canvas is translated `20` to the left, `5` to the top, and then "zoomed-in" to focus on the area which is actually drawn, `105` by `90`.

```html
<svg viewBox="0 0 135 100"></svg>

<svg viewBox="20 5 105 90"></svg>
```

## Optimizing

`d3-omg.svg` optimizes the syntax through [SVGOMG](https://jakearchibald.github.io/svgomg/). The markup is prettified to show how the optimization takes place. The `style` attribute is also preserved, which seems necessary to actually crop the visual in the canvas described by the `viewBox`.

In terms of optimization, it is interesting to note the following:

- `<path>` is preferred to `<rect>` element.

  The sequence of instructions necessary to draw the four lines seems preferable to describe the position and dimensions of the resulting rectangle

  ```html
  <rect y="25" width="30" height="50" />

  <path d="M0 25h30v50H0z" />
  ```

- the instructions in the `d` attribute for `<path>` elements can be stripped of any whitespace separating numbers and letters

- is it often helpful to mix absolute and relative instructions

  Consider for instance the by uppercase and lowercase letters drawing the rectangles (`h` and `H`)

  ```html
  <path d="M0 25h30v50H0z" />
  ```

  Using only relative commands would require a couple of additional keystrokes

  ```html
  <path d="M0 25h30v50h-30z" />
  ```
