# Loading data in D3

D3 has a number of methods to load data from external files. These include `d3.csv()`, `ds.tsv()` and `d3.json()`.

The key thing to bear in mind is that these run *after* the rest of the code, so anything you want to do with the data has to be placed inside the function that's nested inside the parentheses in `d3.csv/tsv/json`.

Here's an example:

```js
//create a variable to store data
var somedata;
//load the data from tsv file
d3.csv("https://raw.githubusercontent.com/paulbradshaw/d3intro/master/docs/data.csv", function(error, data) {
  //assign data to variable
  somedata = data;
  console.log(somedata);
  console.log(somedata[1]);
  console.log(somedata[1].name);
  d3.select("div.newpars").selectAll("p.data").data(somedata).enter().append("p")
    .html(function(d){
      return d.name+": "+d.value;
    })
});

console.log("Note that this code runs before the other console.log commands even though those appear earlier in the code. Also, see how the somedata variable is empty at this point: "+somedata+", yes?");
```

There are 4 `console.log()` commands here - but notice that it's the one *outside* of, and after, `d3.csv()`, that appears first in the console. Once the data is loaded by `d3.csv()` it *then* runs the 3 `console.log` commands inside, and adds the data to the page in a `<p>` tag.

To avoid your `d3.csv()` code getting too long, you can instead create functions elsewhere in the script to store any code you want to run, and just run those functions inside `d3.csv()`, like so:

```js
//create a variable to store data
var somedata;
//load the data from tsv file
d3.csv("https://raw.githubusercontent.com/paulbradshaw/d3intro/master/docs/data.csv", function(error, data) {
  //assign data to variable
  somedata = data;
  console.log(somedata);
  console.log(somedata[1]);
  console.log(somedata[1].name);
  showdata(somedata);
});

var function showdata(somedata) {d3.select("div.newpars").selectAll("p.data").data(somedata).enter().append("p")
  .html(function(d){
    return d.name+": "+d.value;
  })
};

console.log("Note that this code runs before the other console.log commands even though those appear earlier in the code. Also, see how the somedata variable is empty at this point: "+somedata+", yes?");
```
