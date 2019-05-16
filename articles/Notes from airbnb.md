# To convert an `iterable` object to an array, use spreads ... instead of Array.from

```javascript
const foo = document.querySelectorAll('.foo');

// good
const nodes = Array.from(foo);

// best
const nodes = [...foo];
```

# Use Array.from for converting an `array-like` object to an array.

```javascript
const arrLike = { 0: 'foo', 1: 'bar', 2: 'baz', length: 3 };

// bad
const arr = Array.prototype.slice.call(arrLike);

// good
const arr = Array.from(arrLike);
```

# Use Array.from instead of spread ... for mapping over iterables, because it avoids creating an intermediate array.

```javascript
// bad
const baz = [...foo].map(bar);

// good
const baz = Array.from(foo, bar);
```

# Use exponentiation operator ** when calculating exponentiations

```javascript
// bad
const binary = Math.pow(2, 10);

// good
const binary = 2 ** 10;
```

