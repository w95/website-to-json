# Website to json converter (wtj)

This tool converts each website to understandable JSON by jQuery selectors.

## Installation

```bash
$ npm install website-to-json --save
```

## Getting started

### Examples

#### Stack Overflow

```js
var wtj = require('website-to-json')
wtj.extractData('http://stackoverflow.com/questions/3207418/crawler-vs-scraper', {
  fields: ['data'],
  parse: function($) {
    return {
      title: $("h1").text(),
      keywords: $('.post-taglist a').map(function(val) {
        return $(this).text()
      }).get()
    }
  }
})
.then(function(res) {
  console.log(JSON.stringify(res, null, 2));
})
```

Response

```js
{
  "data": {
    "title": "crawler vs scraper",
    "keywords": [
      "web-crawler",
      "terminology",
      "scraper"
    ]
  }
}
```

#### IMDB

```js
var trim = require('trim')
var wtj = require('website-to-json')

wtj.extractData('http://www.imdb.com/title/tt0111161', {
  fields: ['data'],
  parse: function($) {
    return {
      title: trim($(".title_wrapper h1").text()),
      image: $(".poster img").attr('src'),
      summary: trim($(".plot_summary .summary_text").text())
    }
  }
})
.then(function(res) {
  console.log(JSON.stringify(res, null, 2));
})
```

Response

```js
{
  "data": {
    "title": "The Shawshank Redemption (1994)",
    "image": "https://images-na.ssl-images-amazon.com/images/M/MV5BODU4MjU4NjIwNl5BMl5BanBnXkFtZTgwMDU2MjEyMDE@._V1_UX182_CR0,0,182,268_AL_.jpg",
    "summary": "Two imprisoned men bond over a number of years, finding solace and eventual redemption through acts of common decency."
  }
}
```

#### IMDB many URL's

```js
var wtj = require('website-to-json');
var trim = require('trim');

Promise.all([
  'http://www.imdb.com/title/tt0111161',
  'http://www.imdb.com/title/tt0137523',
  'http://www.imdb.com/title/tt0068646'
])
.map(function(url) {
  return wtj.extractUrl(url, {
    fields: ['data'],
    parse: function($) {
      return {
        title: trim($(".title_wrapper h1").text()),
        image: $(".poster img").attr('src')
      }
    }
  })
}, {concurrency: 1})
.then(function(res) {
  console.log(JSON.stringify(res, null, 2));
})
```

## Nightmare.js

- <a href="https://github.com/itemsapi/website-to-json/blob/master/docs/NIGHTMARE.md">Integration with Nightmare</a>

## CLI

```bash
$ sudo npm install website-to-json -g
$ wtj twitter.com/itemsapi
```
