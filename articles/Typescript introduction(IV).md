# Modules

## Export 

### Export declaration

```javascript
export const num = 1;
```

### Export statement

```javascript

// a.js
const num = 1;
const str = 'export';
export { num };
export { str as exportStr }; // export renaming
```

### Export with default

```javascript
// b.js
export default function toString(obj) {
  return obj.toString();
}
```

### Re-exports

```javascript
export { num, exportStr } from './a.js'
// equivalent to 
export * from './a.js'
```

## Import 

```javascript
import { num, exportStr } from './a.js';
import * as util from './a.js'


import toString from './b.js';
// equivalent to
import { default as toString } from './b.js'
```

## `module` in tsconfig

Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', or 'ESNext'.

# Namespace

> Ambient declarations are written using the declare keyword and can declare variables, functions, classes, enums, namespaces, or modules.

## Ambient modules

```javascript
// node.d.ts
declare module "url" {
  export interface Url {
    protocol?: string;
    hostname?: string;
    pathname?: string;
  }

  export function parse(urlStr: string, parseQueryString?): Url;
}
```

```javascript
/// <reference path="node.d.ts"/>
import * as URL from "url";
let myUrl = URL.parse("https://github.com/n0rush");
```

## Shorthand ambient modules

```javascript
declare module "new-module"; // All imports from a shorthand module will have the any type.
```

## Wildcard module declarations

```javascript
declare module "json!*" {
  export const version: string;
  const value: any;
  export default value;
}
```

cant only do

```javascript
import { version } from 'example.json'
import data from 'example.json'
import { data } from 'example.json' // error 
```

## Namespaces produce values

```javascript

namespace N {
  let str = "hello world";
  export function fn() {
    return str;
  }
}

N.fn();
N.str;  // Error, str is not exported
```

The above `M` declaration is equivalent to the folowing in compiled code:

```javascript
var N;
(function (N) {
  var str = "hello world";
  function fn() {
    return str;
  }
  N.fn = fn;
})(N || (N = {}));
N.fn();
N.str; // Error, str is not exported
```


# Module resolution

```javascript
import { a } from 'pathOfModuleA';
```

* First, the compiler will try to locate a file that represents the imported module. To do so the compiler follows one of two different strategies: Classic or Node, which is specified with `moduleResolution` in tsconfig.
* If no match is found and the module name is non-relative, then the compiler will attempt to locate an ambient module declaration. 
* A non-relative import can be resolved relative to baseUrl, or through path mapping. They can also resolve to ambient module declarations. Use non-relative paths when importing any of your external dependencies.

## Base Url

Setting baseUrl informs the compiler where to find modules. All module imports with non-relative names are assumed to be relative to the baseUrl. Note that relative module imports are not impacted by setting the baseUrl, as they are always resolved relative to their importing files.

## Path mapping, equivalent to alias in other tools

```javascript
{
  "compilerOptions": {
    "baseUrl": ".", // This must be specified if "paths" is set.
    "paths": {
      "jquery": ["node_modules/jquery/dist/jquery"] // This mapping is relative to "baseUrl"
    }
  }
}
```

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.
