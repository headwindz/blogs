# undefined

I used to use `myVar == null` to check the existence of a variable. This expression can detect the case where the variable is `null` or `undefined`.

There are two cases where a variable can result in `undefined`:

* The variable is NOT defined.
* The variable is defined but NOT initialized. E.g.

```javascript
let a;
```

# Problem

If a variable is referenced but NOT defined yet in the context, a `Uncaught ReferenceError` will be thrown.

```
// Uncaught ReferenceError: notCreatedVariable is not defined
console.log(notCreatedVariable == null);
```

# Solution

Use `typeof`

```
console.log(typeof notCreatedVariable === 'undefined');
```

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.