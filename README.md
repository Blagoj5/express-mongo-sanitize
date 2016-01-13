# Express Mongoose Sanitize

Express 4.x middleware which sanitizes user-supplied data to prevent MongoDB Operator Injection.

## Installation

``` bash
npm install express-mongo-sanitize
```

## Usage

Add as a piece of express middleware, before defining your routes.

``` js
var express = require('express'),
    bodyParser = require('body-parser'),
    mongoSanitize = require('express-mongo-sanitize');

var app = express();

app.use(bodyParser.urlencoded({extended: true}));
app.use(bodyParser.json());

// To remove data, use:
app.use(mongoSanitize());

// Or, to replace prohibited characters with _, use:
app.use(mongoSanitize({
  replaceWith: '_'
}))

```

## What?

This module searches for any keys in objects that begin with a `$` sign or contain a `.`, from `req.body`, `req.query` or `req.params`. It can then either:

- completely remove these keys and associated data from the object, or
- replace the prohibited characters with another allowed character.

The behaviour is governed by the passed option, `replaceWith`. Set this option to have the sanitizer replace the prohibited characters with the character passed in.

See the spec file for more examples.

## Why?

Object keys starting with a `$` or containing a `.` are _reserved_ for use by MongoDB as operators. Without this sanitization,  malicious users could send an object containing a `$` operator, or including a `.`, which could change the context of a database operation. Most notorious is the `$where` operator, which can execute arbitrary JavaScript on the database.

The best way to prevent this is to sanitize the received data, and remove any offending keys, or replace the characters with a 'safe' one.

## Credits

Inspired by [mongo-sanitize](https://github.com/vkarpov15/mongo-sanitize).

## License

MIT