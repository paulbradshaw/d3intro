# First things to do in D3

## Import the D3 library from CDNJS

You can either download the latest version of the D3 library, or link to an online version. The advantage of linking to an online version on a CDN service is that it saves space and speed (it's served quicker). Here, is the code to pull in the D3 library from CDNJS - version 4.10.0 at the time of writing, but [check the site](https://cdnjs.com/#) for the latest version. This should be placed within your `<head>` tags:

```html
<head>
  <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/d3/4.10.0/d3.min.js"></script>
</head>
```

## Select, add, and change HTML using `.select()` and `.append()`

D3 selects, adds, and customises elements on the HTML page using a number of methods:

* The `.select()` and `.selectAll()` methods select HTML tags that you want to change or add. `.select()` only selects the *first* match, while `.selectAll()` obviously selects more than one. These work just like CSS, so to select something in `<p class="story">` you might write `.select("p.story")`
* D3 can *add* child elements using `.append()`. For example you might select a div, and append within that a new paragraph, or SVG shape, like so: `.append("p")`
* You can change *attributes* using `.attr()` - with an attribute-value pair separated by commas like so: `.attr("class","mything")` or `.attr("height", 100)` (note that the first example has a string value, and the second a numerical value without quotation marks).
* You can add CSS *styling* with `.style()` in a similar way, e.g. `select("a").style("color", "red");`
* You can add *content* to elements using `.text()` (if you only want to change the text inside a tag) and `.html()` (if you want to add text and HTML tags)
* You can add *animation* using `.transition()`

You can test some of the basic concepts quickly in the console by typing this:

```js
d3.select("body").append("h1");
```

You can specify text content with `.text()` like so:

```js
d3.select("body").append("h1").text("My headline");
```

To add text and HTML try this:

```js
d3.select("body").append("p").html("This is a sentence with a <strong>HTML tag</strong> in the middle.");
```

To add attributes such as an image source, hyperlink reference, width, height, class or id, try a line like this (note you must already have a `<p>` tag in the page for it to work):

```js
d3.select("p").append("img").attr("src","https://onlinejournalismblog.files.wordpress.com/2013/08/ojb_logojd_960x250.jpg");
```

To use the same code within your HTML, add the following script:

```html
<body>
  <p>A little introduction</p>
  <script type="text/javascript">
  d3.select("body").append("h1").text("My headline");
  d3.select("body").append("p").html("This is a sentence with a <strong>HTML tag</strong> in the middle.");
  d3.select("p").append("img").attr("src","https://onlinejournalismblog.files.wordpress.com/2013/08/ojb_logojd_960x250.jpg");
  </script>
</body>
```

## Create a shape (SVG)

An SVG (Scalable Vector Graphic) is a way of describing a shape. It uses tags just like HTML, and so can be appended in the same way as the tags above, but it needs particular attention to attributes:

* First `.append()` a parent `<svg>` tag
* Then, within that, another `.append()` to add a child tag for the shape you want to create (`<rect>` for a rectangle, `<circle>`, `<ellipse>`, `<line>`, and so on)
* And finally, use the `.attr()` method to specify attributes such as the `width`, `height`, `fill`, x and y coordinates, and so on. Note also that the parent `<svg>` tag will need a width and height big enough to contain the shape.

Here's some code which uses D3 to append an `<svg>` tag with a width and height attribute of 100 pixels, and then within that a `<rect>` tag with its own width and height:

```html
<body>
  <p>A little introduction</p>
  <script type="text/javascript">
  d3.select("body").append("svg").attr("width", 100).attr("height", 100).append("rect").attr("width", 40).attr("height", 80);
  </script>
</body>
```

Because spaces don't matter, we can break this across lines to make it easier to understand:

```html
<body>
  <p>A little introduction</p>
  <script type="text/javascript">
  d3.select("body")
    .append("svg")
      .attr("width", 100).attr("height", 100)
      .append("rect")
        .attr("width", 40).attr("height", 80);
  </script>
</body>
```

## Create a div bar chart

The first D3 chart is almost always a bar chart, because it can be created through simply making a div tag for each value, and colouring it in. Here's some example code with comments explaining what each line does. All you need in the HTML is the tag `<div class="fillmeup"></div>` for it to target.

```js
//this creates an array variable to store data
var somedata = [10,30,60]
//This selects all div tags - there is  none so it will then create them
d3.select('.fillmeup')
  .selectAll('div')
//This grabs the variable created earlier.
  .data(somedata)
  .enter()
//Because there are 3 elements, this will create 3 placeholder elements
.append('div')
//This function assigns each value to d  and multiplies it, adds 'px', before inserting it as the value for 'width'
  .style("width", function(d){
  return d * 10 + "px";
})
.style("background-color", "red")
.style("margin", "5px")
.text(function(d){
  return d;
})
```

## Create a scale

When we get a lot of data values, rather than sizing each bar (div tag) by multipying each data value, it's better to factor in what sort of range of values they use.

This is typically done by finding out what the maximum value is, with 'd3.max()', and using that to create a 'domain'. [This piece explaining the concepts of domain, range and scales](http://javascript.tutorialhorizon.com/2015/01/17/d3-fundamentals-understanding-domain-range-and-scales-in-d3js/) helpfully sums up a domain as:

> "The boundaries within which your data lies"

To establish those boundaries you need to identify what your maximum value is: you can do this using `d3.max()`, like so:

`d3.max(somedatavariable)`

That then needs to be included in a larger piece of code creating a new function variable which can be used to calculate the domain:

```js
var x = d3.scaleLinear()
    .domain([0, d3.max(somedata)])
    .range([0, 420]);
```

The first thing to explain here is that `d3.scaleLinear()` is a *D3 function* - so when it is assigned to the variable `x`, that means that `x` is *also* a function: you can type `x(30)` in your console and should be able to see what that function returns as a result. How can it return something when `return` is not in your code? Because it's in the code for `scaleLinear` which you are using. All that you are doing here is *customising* that function with your own specifications: how high does your data go (`domain`), and how big is your canvas (`range`). Your new customised version of scaleLinear is now stored as `x`.

Now to explain a bit more about those two specifications: the `domain` part of this script creates one range: from 0 to your data's highest value (60 in the case earlier). The `range` part creates another: the scope of the canvas you want to draw on. When given a number, `x` returns the equivalent position on your canvas of a particular number. For example, 30 is halfway between 0 and 60 (the `domain` of your dataset), so the value returned will be 210 - halfway between 0 and 420 (the `range` you want to draw on).

Note that D3 now uses `d3.scaleLinear()`, while v3 - and therefore many tutorials - used `d3.scale.linear()`. You can [find out more about D3's scale functions in this post](http://d3indepth.com/scales/).

That `x` variable is then used within the code to size divs instead of multiplying it by an arbitrary number, like so:

```js
.style("width", function(d){
  return x(d) + "px";
})
```

Here's the HTML in full that uses that technique to create a bar chart:

```html
<div class="fillmeup"></div>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/d3/4.10.0/d3.min.js"></script>
<script>
//this creates an array variable to store data
var somedata = [10,30,60];
//this calculates the range of values in the data - which can then be used below
var x = d3.scaleLinear()
  .domain([0, d3.max(somedata)])
  .range([0, 600]);

d3.select('.fillmeup')
.selectAll('div')
//This grabs the variable created earlier.
.data(somedata)
.enter()
//Because there are 3 elements, this will create 3 placeholder elements
.append('div')
//This function assigns each value to d and multiplies it, adds 'px', before inserting it as the value for 'width'
.style("width", function(d){
return x(d) + "px";
})
.style("background-color", "red")
.style("margin", "5px")
.text(function(d){
return d;
})
</script>
```
