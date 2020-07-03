Project showcasing SVG attributes and CSS based animation

This is a rather advanced project when learning SVG syntax. It makes sense to include it after **Google Photos and the use Element**, since it relies on the `<use>` element once more, but there are also several features with a very specific use case. Consider the `textLength` attribute, included on both the `<text>` and `<textPath>` element to ensure the value is picked up with more browser support.

Adding a CSS animation also causes an extra layer of complexity, as some elements/attributes are included just to animate the logo. Consider the solid line included in the `#mask__line` mask to hide and then show the dashed variant in full.
