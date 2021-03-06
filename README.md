# util.obj-cycle

> Monkey patched additional JSON functionality for cyclical objects

[![build](https://github.com/jmquigley/util.obj-cycle/workflows/build/badge.svg)](https://github.com/jmquigley/util.obj-cycle/actions)
[![analysis](https://img.shields.io/badge/analysis-tslint-9cf.svg)](https://palantir.github.io/tslint/)
[![code style: prettier](https://img.shields.io/badge/code_style-prettier-ff69b4.svg?style=flat-square)](https://github.com/prettier/prettier)
[![testing](https://img.shields.io/badge/testing-jest-blue.svg)](https://facebook.github.io/jest/)
[![NPM](https://img.shields.io/npm/v/util.obj-cycle.svg)](https://www.npmjs.com/package/util.obj-cycle)

This module is a wrapper for cyclical object resolution using Douglas Crockford's [JSON-js cycle code](https://github.com/douglascrockford/JSON-js).  It [monkey patches](https://en.wikipedia.org/wiki/Monkey_patch) two functions:

- JSON.decycle
- JSON.retrocycle


# Installation

This module uses [yarn](https://yarnpkg.com/en/) to manage dependencies and run scripts for development.

To install as an application dependency with cli:

```
$ yarn add --dev util.obj-cycle
```

To build the app and run all tests:

```
$ yarn run all
```


# Usage

To use this module, just use require to [monkey patch](https://en.wikipedia.org/wiki/Monkey_patch) it into the environment:

```javascript
require('util.obj-cycle');
```

To remove the cycles from an object use `JSON.decycle()`:

```javascript
	const i = {a: "x", ref: null};
	const j = {b: "y", ref: null};
	const k = {c: "z", ref: null};
	i.ref = j;
	j.ref = k;
	k.ref = i;

	let s: string = JSON.stringify(JSON.decycle(i));

    /// s ->  {"a":"x","ref":{"b":"y","ref":{"c":"z","ref":{"$ref":"$"}}}}
```

The process can be reversed on the object with `JSON.retrocycle`:

```javascript
	let obj = JSON.retrocycle(JSON.parse(s));
```

The functions can also be included directly without the monkey patching by using import:

```javascript
import {decycle, retrocycle} from "util.obj-cycle";
```

This is the same underlying code.  Just a differnt way to do the same thing.
