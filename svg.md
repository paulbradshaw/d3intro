# SVGs

Much of D3 involves selecting or appending an empty `<svg>` tag in a HTML page, and appending child elements to create a chart.

At its most basic you can add a single shape, but charts typically need more. To do that you might append child `<g>` elements. These [are described here](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/g) as "a container used to group other SVG elements" - for example, a series of rectangles (the `<rect>` tag) for each bar in a bar chart.

Those elements can then be positioned using the `transform` property, [explained here](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/transform). `transform` can be used to specify coordinates using the `translate`, like so:

`transform="translate(0,50)"`

As in CSS positioning, `translate` works from the upper corner out, so `0,0` would be at the top left of the page, `0,100` would be 100 pixels in from the left, and `100,0` would be 100 pixels down from the top.

Also useful is `scale` like so:

`transform="scale(50,50)"`

And rotate:

`transform="rotate(45)"`

Multiple transformations can be specified, all within one string, with parameters in parentheses, like so:

`transform="translate(0,50) scale(50,50), rotate(45)"`
