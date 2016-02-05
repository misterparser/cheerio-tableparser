# cheerio-tableparser

Most popular cheerio plugin in the world! Port of [jquery.tableparser](https://github.com/siuying/jquery.tableparser) plugin to cheerio.

## Installation
`npm install --save cheerio cheerio-tableparser`

## Introduction
Parsing HTML table could be difficult when its structure contains colspan or rowspan.
Cheerio-tableparser parse HTML tables, group them by columns, with colspan and rowspan respected.

### Example table
    |-----------------------------|
    |  A  |  B  |  C  |  D  |  E  |
    |-----------------------------|
    |     |  2a |  3a |  4a |  5a |
    |     |-----------------------|
    |     |           |  4b |  5b |
    |     |     2b    |-----------|
    |  1a |           |     |  5c |
    |     |-----------|  4c |-----|
    |     |     |  3d |     |  5d |
    |     |  2d |-----------------|
    |     |     |  3e |  4e |  5e |
    |-----------------------------|


```js
var cheerio = require('cheerio'),
    cheerioTableparser = require('cheerio-tableparser');

$ = cheerio.load("<table id='complex'> \
      <tr><td>A</td><td>B</td><td>C</td><td>D</td><td>E</td></tr> \
      <tr><td rowspan='5'>1a</td><td>2a</td><td>3a</td><td>4a</td><td>5a</td></tr> \
      <tr><td rowspan='2' colspan='2'>2b</td><td>4b</td><td>5b</td></tr> \
      <tr><td rowspan='2'>4c</td><td>5c</td></tr> \
      <tr><td rowspan='2'>2d</td><td>3d</td><td>5d</td></tr> \
      <tr><td>3e</td><td>4e</td><td>5e</td></tr> \
    </table>");

cheerioTableparser($);
var data = $("#complex").parsetable(true, true, true);
//data = >
//[ [ 'A', '1a', '1a', '1a', '1a', '1a' ],
//  [ 'B', '2a', '2b', '2b', '2d', '2d' ],
//  [ 'C', '3a', '2b', '2b', '3d', '3e' ],
//  [ 'D', '4a', '4b', '4c', '4c', '4e' ],
//  [ 'E', '5a', '5b', '5c', '5d', '5e' ] ]
```



## Features
&#9786; __Easy to use:__
You just need cheerio and cheerio-tablerparser to parse complex tables, as easy as JSON.parse(). You get array of columns.

&#9881; __Configurable:__
Cheerio-tableparser can duplicate empty cells in rows or columns or even both. Result can be text or html.

&#991; __Fast:__
As fast as cheerio is.

## When I should use Cheerio-tableparser

Cheerio-tableparser used only for tables created with “table” tag. In other unusual situations you should use other solutions. For example css formated text looks like table is not affordable.

## How it works

```js
var cheerio = require('cheerio'),
    cheerioTableparser = require('cheerio-tableparser');

$ = cheerio.load("<table></table>");

cheerioTableparser($);
data = $("table").parsetable();
// data = > []
```

### .parsetable(dupCols, dupRows, textMode)
- `dupCols`: if true empty cells will be copy of left filled column. If false empty cell. Default: false.
- `dupRows`: if true empty cells will be copy of upper filled row. If false empty cell. Default: false.
- `textMode `: if true result will be text same as cell $("td").text().trim(). If false result will be HTML same as cell $("td").html(). Default: false.

#### Load data and add plugin
```js
var cheerio = require('cheerio'),
    cheerioTableparser = require('cheerio-tableparser');

$ = cheerio.load("<table id='complex'> \
      <tr><td>A</td><td>B</td><td>C</td><td>D</td><td>E</td></tr> \
      <tr><td rowspan='5'>1a</td><td>2a</td><td>3a</td><td>4a</td><td>5a</td></tr> \
      <tr><td rowspan='2' colspan='2'>2b</td><td>4b</td><td>5b</td></tr> \
      <tr><td rowspan='2'>4c</td><td>5c</td></tr> \
      <tr><td rowspan='2'>2d</td><td>3d</td><td>5d</td></tr> \
      <tr><td>3e</td><td>4e</td><td>5e</td></tr> \
    </table>");

cheerioTableparser($);
```

#### Default usage no copy
```js
var data = $("#complex").parsetable(); // same as .parsetable(false, false, false);
//data = >
//[ [ 'A', '1a', '', '', '', '' ],
//  [ 'B', '2a', '2b', '', '2d', '' ],
//  [ 'C', '3a', '', '', '3d', '3e' ],
//  [ 'D', '4a', '4b', '4c', '', '4e' ],
//  [ 'E', '5a', '5b', '5c', '5d', '5e' ] ]
```

#### Copy data ONLY from UPPER row with filled cell

```js
var data = $("#complex").parsetable(false, true, false);
//data = >
//[ [ 'A', '1a', '1a', '1a', '1a', '1a' ],
//  [ 'B', '2a', '2b', '2b', '2d', '2d' ],
//  [ 'C', '3a', '', '', '3d', '3e' ],
//  [ 'D', '4a', '4b', '4c', '4c', '4e' ],
//  [ 'E', '5a', '5b', '5c', '5d', '5e' ] ]
```

#### Copy data ONLY from LEFT column with filled cell

```js
var data = $("#complex").parsetable(true, false, false);
//data = >
//[ [ 'A', '1a', '', '', '', '' ],
//  [ 'B', '2a', '2b', '', '2d', '' ],
//  [ 'C', '3a', '2b', '', '3d', '3e' ],
//  [ 'D', '4a', '4b', '4c', '', '4e' ],
//  [ 'E', '5a', '5b', '5c', '5d', '5e' ] ]
```

#### Copy data BOTH from LEFT column AND UPPER row with filled cell

```js
var data = $("#complex").parsetable(true, true, false);
//data = >
//[ [ 'A', '1a', '1a', '1a', '1a', '1a' ],
//  [ 'B', '2a', '2b', '2b', '2d', '2d' ],
//  [ 'C', '3a', '2b', '2b', '3d', '3e' ],
//  [ 'D', '4a', '4b', '4c', '4c', '4e' ],
//  [ 'E', '5a', '5b', '5c', '5d', '5e' ] ]
```

### Example HTML table

    |---------------------------------------------------------------------|
    |        <strong>A</strong>        |        <strong>B</strong>        |
    |---------------------------------------------------------------------|
    | <div class='table-text'>1a</div> | <div class='table-text'>2a</div> |
    |---------------------------------------------------------------------|


#### Load data and add plugin
```js
var cheerio = require('cheerio'),
    cheerioTableparser = require('cheerio-tableparser');

$ = cheerio.load("<table id='html'> \
    <tr><td><strong>A</strong></td><td><strong>B</strong></td></tr> \
    <tr><td><div class='table-text'>1a</div></td><td><div class='table-text'>2a</div></td></tr> \
    </table>");

cheerioTableparser($);
```

#### Return data as HTML
```js
var data = $("#html").parsetable(false, false, false);
//data = >
//[ [ '<strong>A</strong>', '<div class="table-text">1a</div>' ],
//  [ '<strong>B</strong>', '<div class="table-text">2a</div>' ] ]

//To make jquery object from string use $('<div>' + data[0][0]+ '</div>');
//additional "div" needed if data[0][0] is not valid html string.

//Example:
var strong = $('<div>' + data[0][0] + '</div>').find('strong').text();
//strong = > 'A'

var tableText = $('<div>' + data[0][1] + '</div>').find('.table-text').text();
//tableText = > '1a'

```

#### Return data as Text
```js
var data = $("#html").parsetable(false, false, true);
//data = >
//[ [ 'A', '1a' ],
//  [ 'B', '2a' ] ]
```

## License

(The MIT License)

Copyright (c) 2011 Francis Chong

Copyright (c) 2016 Mister Parser

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
