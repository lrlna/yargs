yargs
========

Yargs be a node.js library fer hearties tryin' ter parse optstrings.

With yargs, ye be havin' a map that leads straight to yer treasure! Treasure of course, being a simple option hash.

[![Build Status][travis-image]][travis-url]
[![Coverage Status][coveralls-image]][coveralls-url]
[![NPM version][npm-image]][npm-url]
[![Windows Tests][windows-image]][windows-url]
[![js-standard-style][standard-image]][standard-url]
[![standard-version][standard-version-image]][standard-version-url]
[![Gitter][gitter-image]][gitter-url]

> Yargs is the official successor to optimist. Please feel free to submit issues and pull requests. If you'd like to contribute and don't know where to start, have a look at [the issue list](https://github.com/yargs/yargs/issues) :)

table of contents
=================

1. [About](#about)
2. [Projects using yargs](#projects-using-yargs)
3. [Example](#example)
4. [Methods](./docs/api.md)
5. [Usage](./docs/usage.md)
6. [Parsing tricks](#parsing-tricks)
   - [stop parsing](#stop-parsing)
   - [negate-parsing](#negate-parsing)
   - [numbers](#numbers)
   - [duplicates](#duplicates)
   - [dot notation](#dot-notation)
   - [short numbers](#short-numbers)
7. [Contributing](#contribute)
8. [Installation](#installation)
9. [Configuration](#configuration)
10. [Inspired by](#inspired-by)

parsing tricks
==============

stop parsing
------------

Use `--` to stop parsing flags and stuff the remainder into `argv._`.

    $ node examples/reflect.js -a 1 -b 2 -- -c 3 -d 4
    { _: [ '-c', '3', '-d', '4' ],
      a: 1,
      b: 2,
      '$0': 'examples/reflect.js' }

negate fields
-------------

If you want to explicitly set a field to false instead of just leaving it
undefined or to override a default you can do `--no-key`.

    $ node examples/reflect.js -a --no-b
    { _: [], a: true, b: false, '$0': 'examples/reflect.js' }

numbers
-------

Every argument that looks like a number (`!isNaN(Number(arg))`) is converted to
one. This way you can just `net.createConnection(argv.port)` and you can add
numbers out of `argv` with `+` without having that mean concatenation,
which is super frustrating.

duplicates
----------

If you specify a flag multiple times it will get turned into an array containing
all the values in order.

    $ node examples/reflect.js -x 5 -x 8 -x 0
    { _: [], x: [ 5, 8, 0 ], '$0': 'examples/reflect.js' }

dot notation
------------

When you use dots (`.`s) in argument names, an implicit object path is assumed.
This lets you organize arguments into nested objects.

    $ node examples/reflect.js --foo.bar.baz=33 --foo.quux=5
    { _: [],
      foo: { bar: { baz: 33 }, quux: 5 },
      '$0': 'examples/reflect.js' }

short numbers
-------------

Short numeric `-n5` style arguments work too:

    $ node examples/reflect.js -n123 -m456
    { _: [], n: 123, m: 456, '$0': 'examples/reflect.js' }

installation
============

With [npm](https://github.com/npm/npm), just do:

    npm install yargs

or clone this project on github:

    git clone http://github.com/yargs/yargs.git

To run the tests with npm, just do:

    npm test

configuration
=============

Using the `yargs` stanza in your `package.json` you can turn on and off
some of yargs' parsing features:

```json
{
  "yargs": {
    "short-option-groups": true,
    "camel-case-expansion": true,
    "dot-notation": true,
    "parse-numbers": true,
    "boolean-negation": true
  }
}
```

See the [yargs-parser](https://github.com/yargs/yargs-parser#configuration) module
for detailed documentation of this feature.

inspired by
===========

This module is loosely inspired by Perl's
[Getopt::Casual](http://search.cpan.org/~photo/Getopt-Casual-0.13.1/Casual.pm).

[travis-url]: https://travis-ci.org/yargs/yargs
[travis-image]: https://img.shields.io/travis/yargs/yargs/master.svg
[coveralls-url]: https://coveralls.io/github/yargs/yargs
[coveralls-image]: https://img.shields.io/coveralls/yargs/yargs.svg
[npm-url]: https://www.npmjs.com/package/yargs
[npm-image]: https://img.shields.io/npm/v/yargs.svg
[windows-url]: https://ci.appveyor.com/project/bcoe/yargs-ljwvf
[windows-image]: https://img.shields.io/appveyor/ci/bcoe/yargs-ljwvf/master.svg?label=Windows%20Tests
[standard-image]: https://img.shields.io/badge/code%20style-standard-brightgreen.svg
[standard-url]: http://standardjs.com/
[standard-version-image]: https://img.shields.io/badge/release-standard%20version-brightgreen.svg
[standard-version-url]: https://github.com/conventional-changelog/standard-version
[gitter-image]: https://img.shields.io/gitter/room/nwjs/nw.js.svg?maxAge=2592000
[gitter-url]: https://gitter.im/yargs/Lobby?utm_source=share-link&utm_medium=link&utm_campaign=share-link
