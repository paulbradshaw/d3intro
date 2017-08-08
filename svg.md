# SVGs

Much of D3 involves selecting or appending an empty `<svg>` tag in a HTML page, and appending child elements to create a chart.

At its most basic you can add a single shape, but charts typically need more. To do that you might append child `<g>` elements. These [are described here](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/g) as "a container used to group other SVG elements" - for example, a series of rectangles (the `<rect>` tag) for each bar in a bar chart.

Below, for example is the HTML for a SVG [from a tutorial by Mike Bostock](https://bost.ocks.org/mike/bar/2/): the `<svg>` tag contains 3 child `<g>` tags, each of which contains a `<rect>` and a `<text>` tag. All of the tags have various attributes:

```html
<svg class="chart" width="420" height="120">
  <g transform="translate(0,0)">
    <rect width="40" height="19"></rect>
    <text x="37" y="9.5" dy=".35em">4</text>
  </g>
  <g transform="translate(0,20)">
    <rect width="80" height="19"></rect>
    <text x="77" y="9.5" dy=".35em">8</text>
  </g>
  <g transform="translate(0,40)">
    <rect width="150" height="19"></rect>
    <text x="147" y="9.5" dy=".35em">15</text>
  </g>
</svg>
```

## The `transform` attribute

One attribute that needs particular explanation is `transform`. This is how each element within an `svg` can be positioned to ensure that they do not overlap. It is [explained here in more detail](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/transform): the main use is how `transform` can be used to specify coordinates for the element using `translate`, like so:

`transform="translate(0,50)"`

As in CSS positioning, `translate` works from the upper corner out, so `0,0` would be at the top left of the page, `0,100` would be 100 pixels in from the left, and `100,0` would be 100 pixels down from the top.

Also useful is `scale` like so, which 'scales' an element to be larger or smaller:

`transform="scale(50,50)"`

And rotate:

`transform="rotate(45)"`

Multiple transformations can be specified, all within one string, with parameters in parentheses, like so:

`transform="translate(0,50) scale(50,50), rotate(45)"`
