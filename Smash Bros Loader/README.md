The project quite uncomplicated, but includes a few important features of SVG syntax. Most notably:

- how to hide/show parts of the canvas with a `<mask>` element.

  Anything with a color value of `hsl(0, 0%, 0%)` is hidden, with a color value of `hsl(0, 0%, 100%)` is shown in full

- how to change the appearance of drawing elements with a `<filter>` element

  The inclusion of a filter is rudimentary so tay the least, and serves more as an introduction.

  ```html
  <filter id="blur">
    <feGaussianBlur stdDeviation="3" />
  </filter>
  ```

  Most elements in the `<defs>` block rely on drawing elements, but `<filter>` depends on its own syntax. Consider `<feGaussianBlur>` instead of `<circle>` and `<path>` elements.

- how the `transform` and the `transform-origin` properties work in the context of SVG elements.

  In the project the rotation applied on the `<path>` elements hinges on the center of the visual.
