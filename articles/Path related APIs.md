```
+-- myProject
|	+-- node_modules
|		+-- lodash
|	+-- src
|   	+-- cwd.js
|		+-- dirname.js
|		+-- require.js
```

switch current working directory into `myProject`

```bash
pwd // '/Users/michaelzheng/Desktop/myProject'
```

## [process.cwd](https://nodejs.org/docs/latest/api/process.html#process_process_cwd)

> The process.cwd() method returns the current working directory of the node.js process. i.e. the directory from which you invoked the `node` command.  

`relative to the process`

```javascript
#!/usr/bin/env node

// cwd.js
process.cwd(); 
// Returns '/Users/michaelzheng/Desktop/myProject'
```

Use case: fetch project config file which is usually set based on project root. e.g. webpack config file.

## [__dirname](https://nodejs.org/docs/latest/api/modules.html#modules_dirname)
> returns the directory name of the directory containing the JavaScript source code file.  

`relative to the source code`

```javascript
#!/usr/bin/env node

// dirname.js
__dirname; 
// Returns '/Users/michaelzheng/Desktop/myProject/src'
```

Use case: get absolute path of a file

## [require.resolve](https://nodejs.org/api/modules.html#modules_require_resolve_request_options)
> use(es) the internal require() machinery to look up the location of a module, but rather than loading the module, just return(s) the resolved filename.  

`works for module resolution`

```javascript

// require.js
require.resolve('lodash'); 
// Returns /Users/michaelzheng/Desktop/myProject/node_modules/lodash/lodash.js
```

Use case: resolve node modules which are not installed on current working directory. 

## [path.resolve](https://nodejs.org/docs/latest/api/path.html#path_path_resolve_paths)

> The path.resolve() method resolves a sequence of paths or path segments into an `absolute path`.  

`always returns absolute path`

```javascript

path.resolve('/foo/bar', './baz');
// Returns: '/foo/bar/baz'

path.resolve('/foo/bar', '/tmp/file/');
// Returns: '/tmp/file'
```

## [path.join](https://nodejs.org/docs/latest/api/path.html#path_path_join_paths)

> The path.join() method joins all given path segments together using the platform-specific separator as a delimiter, then normalizes the resulting path.  

```javascript
path.join('/foo', 'bar', 'baz/asdf', 'quux', '..'); 
// Returns '/foo/bar/baz/asdf'

path.join('foo', 'bar', 'baz/asdf', 'quux', '..'); 
// Returns 'foo/bar/baz/asdf'
```

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.