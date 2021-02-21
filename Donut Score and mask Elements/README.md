# Altmetric and mask Elements

Researching a different topic, I stumbled on a particular visual on the [Altmetric.com](https://www.altmetric.com/) platform (a platform which offers information for where a publication is actually shared). The visual is the one designed as the [_attention score_](https://www.altmetric.com/about-our-data/the-donut-and-score/), and has a nice twist on the logo for the website; instead of a full circle, the colorful slices are tucked in, each behind the one which follows. Here I try to recreate the look with `<circle>` and `<mask>` elements.

## Donut

The donut portion is rapidly solved with a `<mask>` element, hiding everything in the central portion of the `<svg>` element with a `<circle>` element with a `black` fill.

```html
<mask id="mask-center">
  <circle r="50" fill="white" />
  <circle r="30" fill="black" />
</mask>
```

The mask is then applied to the group element `<g>` housing the different slices.

```html
<g mask="url(#mask-center)">
  <!-- draw slices here -->
</g>
```

_Please note:_ the specific mask is omitted in the intermediate demo, to focus on the design of the slices. It is finally included in `donut-score.svg`

## Slices

One way I found to recreate the "tucked" in slices is by overlapping a series of circles, positioned around the center with an offset.

```html
<circle r="30" transform="rotate(0) translate(20 0)" />
<circle r="30" transform="rotate(45) translate(20 0)" />
```

The logic is repeated eight times for the `[0-360]` range, each time with a different `fill` color (see `slices.svg` for a reference), and works with a predictable issue: given the order in the SVG element, the `<circle>`s are drawn on top of one another. The last, pink, circle overlaps the one before it, purple, but also the very first circle, red.

## Masks

The idea is to fix the overlap with additional `<mask>` elements. Here, it is the individual `<circle>` elements which are cropped, to show only a veritable slice.

```html
<mask id="mask-slice">
  <circle r="30" fill="white" />
  <circle r="24" cy="15" fill="black" />
</mask>
```

A radius of `30` matches the radius of the circles drawn after the `<defs>` block, but the radius of `24` and the vertical offset of `15` are chosen purely out of experimentation. By testing different offsets, I found cropping the lowever portion of the circle provides a convenient way to show a reasonable section of the slice (see `masks.svg` for a reference).

## Finishing Touches

Beside the `<mask>` element cropping out the center of the logo, as described in [the _Donut_ section](#Donut), the final version proceeds to add a shadow with a radial gradient.

```html
<radialGradient id="shadow-slice">
  <stop stop-color="hsl(0, 0%, 0%, 0)" offset="0.75" />
  <stop stop-color="hsl(0, 0%, 0%, 1)" offset="1" />
</radialGradient>
```

Instead of repeating the `<circle>` elements then, the visual is defined in the `<defs>` block.

```html
<g id="slice" mask="url(#mask-slice)">
  <circle r="30" />
</g>
```

The group element `<g>` provides a convenient wrapper for the shadow as well.

```html
<g id="slice">
  <circle r="30" />
  <circle r="30" opacity="0.15" fill="url(#shadow-slice)" />
</g>
```

And a more declarative syntax through the `<use>` element.

```html
<g mask="url(#mask-center)">
  <use href="#slice" />
  <!-- other slices -->
</g>
```

Conveniently, the `fill` and `transform` attributes work on `<use>` elements as well.
