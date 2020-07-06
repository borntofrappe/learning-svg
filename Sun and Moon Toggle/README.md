Here I try to recreate the interesting visual and interaction showcased on [joshwcomeau.com](https://joshwcomeau.com/), to toggle between two different color palette.

The palette only changes the color and background property, but the demo is oriented more toward the necessary SVG syntax and the peculiar CSS timing functions.

## Syntax

In `moon.svg` and `sun.svg` you find the visuals for the sun and moon. In the `.html`, the two are merged into a single `<svg>` element, and its features are modified to show either version through `transform` properties.
