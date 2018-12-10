# Reading in Data

The first step in any data processing is getting the data! Here is how to parse in and prepare common input formats using D3.js

## Parsing CSV Files

[D3 has a bunch](https://github.com/d3/d3/blob/master/API.md#fetches-d3-fetch) of filetypes it can support when loading data, and one of the most common is probably plain old CSV (comma separated values).

Let's say you had a csv file with some city data in it:


```bash
cities.csv:

city,state,population,land area
seattle,WA,652405,83.9
new york,NY,8405837,302.6
boston,MA,645966,48.3
kansas city,MO,467007,315.0
```

Use [d3.csv](https://github.com/d3/d3-fetch/blob/master/README.md#csv) to convert it into an array of objects

@@ code=read_data/read_data.01.js @@
@@ code=read_data/read_data.01.out @@

<div class="aside">This code is using d3.js</div>

You can see that the headers of the original CSV have been used as the property names for the data objects. Using `d3.csv` in this manner requires that your CSV file has a header row.

If you look closely, you can also see that the values associated with these properties are all strings. This is probably _not what you want_ in the case of numbers. When loading CSVs and other flat files, you have to do the type conversion.

We will see more of this in other tasks, but a simple way to do this is to use the [+](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Unary_plus) operator (unary plus). `forEach` can be used to iterate over the data array.

@@ code=read_data/read_data.02.js @@
@@ code=read_data/read_data.02.out @@

<div class="aside">This code is using d3.js</div>

[Dot notation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Property_Accessors) is a useful way to access the properties of these data objects. However, if your headers have spaces in them, then you will need to use bracket notation as shown.


This can also be done during the loading of the data, by `d3.csv` directly. This is done by providing an accessor function to `d3.csv`, whose return value will be the individual data objects in our data array.

@@ code=read_data/read_data.03.js @@
@@ code=read_data/read_data.03.out @@

<div class="aside">This code is using d3.js</div>

In this form, you have complete control over the data objects and can rename properties (like `land_area`) and convert values (like `population`) willy-nilly.  On the other hand, you have to be quite explicit about which properties to return. This may or may not be what you are into.

I typically allow D3 to load all the data, and then make modifications in a post-processing step, but it might be more effective for you to be more explicit with the modifications.


## Reading TSV Files

CSV is probably the most common flat file format, but in no way the only one.

I often like to use TSV (tab separated files) - to get around the issues of numbers and strings often having commas in them.

D3 can parse TSV's with [d3.tsv](https://github.com/d3/d3-fetch/blob/master/README.md#tsv).

Here is `animals.tsv`, as an example:

```
name	type	avg_weight
tiger	mammal	260
hippo	mammal	3400
komodo dragon	reptile	150
```
Loading animals.tsv with `d3.tsv`:

@@ code=read_data/read_data.04.js @@
@@ code=read_data/read_data.04.out @@

<div class="aside">This code is using d3.js</div>

## Reading Other Flat Files

In fact, `d3.csv` and `d3.tsv` are only the tip of the iceberg. If you have a non-standard delimited flat file, you can parse them too using [d3.dsv](https://github.com/d3/d3-fetch/blob/master/README.md#dsv)!

For example, here is a pipe-delimited file called `animals_piped.txt`:

```
name|type|avg_weight
tiger|mammal|260
hippo|mammal|3400
komodo dragon|reptile|150
```
We first provide `d3.dsv` with the delimiter, in this case, a pipe (`|`), then read in our file:

@@ code=read_data/read_data.05.js @@
@@ code=read_data/read_data.05.out @@

<div class="aside">This code is using d3.js</div>

## Reading JSON Files

For nested data, or for passing around data where you don't want to mess with data typing, its hard to beat [JSON](http://json.org/).

JSON has become the language of the internet for good reason. Its easy to understand, write, and parse. And with [d3.json](https://github.com/d3/d3-fetch/blob/master/README.md#json) - you too can harness its power.


Here is an example JSON file called `employees.json`:

```
[
 {"name":"Andy Hunt",
  "title":"Big Boss",
  "age": 68,
  "bonus": true
 },
 {"name":"Charles Mack",
  "title":"Jr Dev",
  "age":24,
  "bonus": false
 }
]
```

Loading `employees.json` with `d3.json`:

@@ code=read_data/read_data.06.js @@
@@ code=read_data/read_data.06.out @@

<div class="aside">This code is using d3.js</div>

We can see that, unlike our flat file parsing, numeric types stay numeric. Indeed, a JSON value can be a string, a number, a boolean value, an array, or another object. This allows nested data to be dealt with easily.

## Loading Multiple Files

D3's basic loading mechanism is fine for one file, but starts to get messy as we nest multiple callbacks.

For loading multiple files, we can use [Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) to wait for multiple data sources to be loaded.

@@ code=read_data/read_data.07.js @@
@@ code=read_data/read_data.07.out @@

<div class="aside">This code is using d3.js</div>

Note that inside the `all` method we load two types of files - using two different loading functions - so this is an easy way to mix and match file types.

The method returns an array of our data sources. The first item returns our cities; the second, our animals.


## Next Task

[Combining Data](combine_data.html)

## See Also

- [D3 documentation](https://github.com/d3/d3-fetch)
- [Loading XML with D3](https://github.com/d3/d3-fetch#xml)
- [Loading External SVG with D3](http://bl.ocks.org/mbostock/1014829) - SVG is just XML!
