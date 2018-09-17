## Background

We want to have a function which checks whether an item in JS object representation is a commondity. An item is a commondity if it satisfies one of :

* HAS a price attribute
* HAS a barcode attribute

The following tests should pass:

```javascript
isCommondity({price: 1}); //true
isCommondity({barcode: 'abc'}); //true
isCommondity({a: 'a'}); //false
```

## Round one

It seems to be quite straightfordword to come out with the following solution:

```javascript
function isCommondityV1(comd) {
  return !!comd.price || !!comd.barcode
}
```

The implementation is wrong under some circumstances. `!!obj.prop` is checking the [truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) of the `prop` property of `obj`, instead of checking the existence of the `prop` property. E.g.

```
isCommondityV1({price: 0}); // false
```
while we expect `isCommondityV1` to return true since `{price: 0}` does have the `price` property.

## Round two

[hasOwnProperty](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty)

```javascript
function isCommondityV2(comd) {
  return comd.hasOwnProperty('price') || comd.hasOwnProperty('barcode')
}

isCommondityV2({price: 0}); //true
```

The solution works perfectly in line with the functionality requirement. However, it does suffer from the two problems:

* Flexibility: if we add more checkings later on, we would have to update the implementation body. E.g. when the attribute set extends to be `['price', 'barcode', 'a', 'b']`.

```javascript
function isCommondityV21(comd) {
  return comd.hasOwnProperty('price') || 
          comd.hasOwnProperty('barcode') || 
          comd.hasOwnProperty('a') || 
          comd.hasOwnProperty('b');
}
```

* DRY: We're repeating the operation (i.e `comd.hasOwnProperty`). We can be better off by seperating the variables and encapsulate the constants.


## Round three

```javascript
function isCommondityV3(comd) {
  return ['price', 'barcode'].some(Object.prototype.hasOwnProperty, comd);
}
```

With this implementation, we can easily deal with later change by updating the variables.

```javascript
function isCommondityV31(comd) {
  return ['price', 'barcode', 'a', 'b'].some(Object.prototype.hasOwnProperty, comd);
}
```

Or we can even go one more step further by encapsulating the variables into `config` file. Futher requirement change will result in only changes in `config` file without touching the implementation body :-)

```javascript
//config.js
export COMMONDITY_PROPS = ['price', 'barcode'];
```

```javascript
//commondity.js
import { COMMONDITY_PROPS } from 'config.js'

function isCommondity(comd) {
  return COMMONDITY_PROPS.some(Object.prototype.hasOwnProperty, comd);
}
```

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.
