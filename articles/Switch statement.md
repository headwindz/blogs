Two small techniques/hints for `switch` statement in JS/TS.

# Curly braces after `case`

Curly braces after `case` establishes their own block scope, where you can define local variables.

## Error
```ts
function foo(x: number) {
  switch (x) {
    case 1:
      let a = 1;
      break;
    case 2:
      let a = 2; // error, duplicate variable `a`
      break;
  }
}
```

## Ok

```ts
function foo(x: number) {
  switch (x) {
    case 1: {
      let a = 1;
      break;
    }
    case 2: {
      let a = 2; // ok, each case has its own scope
      break;
    }
  }
}
```

# Is any cases matched?

Make use of `default` to check whether any `case` statement has been matched. E.g.

```ts
function isMatched(x: number) {
  const hasMatch = true;
  switch (x) {
    case 1: {
      // xxxx
      break;
    }
    case 2: {
      // yyyy
      break;
    }
    default: {
      hasMatch = false;
    }
  }
  return hasMatch;
}
```

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.