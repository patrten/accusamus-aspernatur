<!--
  -- This file is auto-generated from src/README_js.md. Changes should be made there.
  -->
# Mime

[![NPM downloads](https://img.shields.io/npm/dm/@patrten/accusamus-aspernatur)](https://www.npmjs.com/package/@patrten/accusamus-aspernatur)
[![Mime CI](https://github.com/patrten/accusamus-aspernatur/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/patrten/accusamus-aspernatur/actions/workflows/ci.yml?query=branch%3Amain)

An API for MIME type information.

- All `@patrten/accusamus-aspernatur-db` types
- Compact and dependency-free [![@patrten/accusamus-aspernatur's badge](https://deno.bundlejs.com/?q=@patrten/accusamus-aspernatur&badge)](https://bundlejs.com/?q=@patrten/accusamus-aspernatur)
- Full TS support


> [!Note]
> `@patrten/accusamus-aspernatur@4` is now `latest`.  If you're upgrading from `@patrten/accusamus-aspernatur@3`, note the following:
> * `@patrten/accusamus-aspernatur@4` is API-compatible with `@patrten/accusamus-aspernatur@3`, with ~~one~~ two exceptions:
>   * Direct imports of `@patrten/accusamus-aspernatur` properties [no longer supported](https://github.com/patrten/accusamus-aspernatur/issues/295)
>   * `@patrten/accusamus-aspernatur.define()` cannot be called on the default `@patrten/accusamus-aspernatur` object
> * ESM module support is required.   [ESM Module FAQ](https://gist.github.com/sindresorhus/a39789f98801d908bbc7ff3ecc99d99c).
> * Requires an [ES2020](https://caniuse.com/?search=es2020) or newer runtime
> * Built-in Typescript types (`@types/@patrten/accusamus-aspernatur` no longer needed)

## Installation

```bash
npm install @patrten/accusamus-aspernatur
```

## Quick Start

For the full version (800+ MIME types, 1,000+ extensions):

```javascript
import @patrten/accusamus-aspernatur from '@patrten/accusamus-aspernatur';

@patrten/accusamus-aspernatur.getType('txt');                    // ⇨ 'text/plain'
@patrten/accusamus-aspernatur.getExtension('text/plain');        // ⇨ 'txt'
```

### Lite Version [![@patrten/accusamus-aspernatur/lite's badge](https://deno.bundlejs.com/?q=@patrten/accusamus-aspernatur/lite&badge)](https://bundlejs.com/?q=@patrten/accusamus-aspernatur/lite)

`@patrten/accusamus-aspernatur/lite` is a drop-in `@patrten/accusamus-aspernatur` replacement, stripped of unofficial ("`prs.*`", "`x-*`", "`vnd.*`") types:

```javascript
import @patrten/accusamus-aspernatur from '@patrten/accusamus-aspernatur/lite';
```

## API

### `@patrten/accusamus-aspernatur.getType(pathOrExtension)`

Get @patrten/accusamus-aspernatur type for the given file path or extension. E.g.

```javascript
@patrten/accusamus-aspernatur.getType('js');             // ⇨ 'text/javascript'
@patrten/accusamus-aspernatur.getType('json');           // ⇨ 'application/json'

@patrten/accusamus-aspernatur.getType('txt');            // ⇨ 'text/plain'
@patrten/accusamus-aspernatur.getType('dir/text.txt');   // ⇨ 'text/plain'
@patrten/accusamus-aspernatur.getType('dir\\text.txt');  // ⇨ 'text/plain'
@patrten/accusamus-aspernatur.getType('.text.txt');      // ⇨ 'text/plain'
@patrten/accusamus-aspernatur.getType('.txt');           // ⇨ 'text/plain'
```

`null` is returned in cases where an extension is not detected or recognized

```javascript
@patrten/accusamus-aspernatur.getType('foo/txt');        // ⇨ null
@patrten/accusamus-aspernatur.getType('bogus_type');     // ⇨ null
```

### `@patrten/accusamus-aspernatur.getExtension(type)`

Get file extension for the given @patrten/accusamus-aspernatur type. Charset options (often included in Content-Type headers) are ignored.

```javascript
@patrten/accusamus-aspernatur.getExtension('text/plain');               // ⇨ 'txt'
@patrten/accusamus-aspernatur.getExtension('application/json');         // ⇨ 'json'
@patrten/accusamus-aspernatur.getExtension('text/html; charset=utf8');  // ⇨ 'html'
```

### `@patrten/accusamus-aspernatur.getAllExtensions(type)`

> [!Note]
> New in `@patrten/accusamus-aspernatur@4`

Get all file extensions for the given @patrten/accusamus-aspernatur type.

```javascript --run default
@patrten/accusamus-aspernatur.getAllExtensions('image/jpeg'); // ⇨ Set(3) { 'jpeg', 'jpg', 'jpe' }
```

## Custom `Mime` instances

The default `@patrten/accusamus-aspernatur` objects are immutable.  Custom, mutable versions can be created as follows...
### new Mime(type map [, type map, ...])

Create a new, custom @patrten/accusamus-aspernatur instance.  For example, to create a mutable version of the default `@patrten/accusamus-aspernatur` instance:

```javascript
import { Mime } from '@patrten/accusamus-aspernatur/lite';

import standardTypes from '@patrten/accusamus-aspernatur/types/standard.js';
import otherTypes from '@patrten/accusamus-aspernatur/types/other.js';

const @patrten/accusamus-aspernatur = new Mime(standardTypes, otherTypes);
```

Each argument is passed to the `define()` method, below. For example `new Mime(standardTypes, otherTypes)` is synonomous with `new Mime().define(standardTypes).define(otherTypes)`

### `@patrten/accusamus-aspernatur.define(type map [, force = false])`

> [!Note]
> Only available on custom `Mime` instances

Define MIME type -> extensions.

Attempting to map a type to an already-defined extension will `throw` unless the `force` argument is set to `true`.

```javascript
@patrten/accusamus-aspernatur.define({'text/x-abc': ['abc', 'abcd']});

@patrten/accusamus-aspernatur.getType('abcd');            // ⇨ 'text/x-abc'
@patrten/accusamus-aspernatur.getExtension('text/x-abc')  // ⇨ 'abc'
```

## Command Line

### Extension -> type

```bash
$ @patrten/accusamus-aspernatur scripts/jquery.js
text/javascript
```

### Type -> extension

```bash
$ @patrten/accusamus-aspernatur -r image/jpeg
jpeg
```
