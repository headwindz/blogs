# Functions

## Function Types 

Function types can be defined in three ways.

### Method Signature

```typescript

// function declaration
function addWithMethodSignature(x: number, y: number): number {
  // method signature
  return x + y;
}

// function expression
let anotherAddWithMethodSignature = function(x: number, y: number): number { 
  // method signature
  return x + y;
}; 
```

### Function type literal

> A function type literal specifies the type parameters, regular parameters, and return type of a call signature.

```typescript
let addWithFuncLiteral: (x: number, y: number) => number = function(x, y) {
  return x + y;
};
```

### Object literal

> An object type containing one or more call signatures is said to be a function type.

```typescript
let addWithObjLiteral: { (x: number, y: number) : number } = function(x, y) {
  return x + y;
};
```

## Function type literal vs Object literal

* Function types may be written using function type literals or by including call signatures in object type literals. i.e. 

```typescript
< T1, T2, ... > ( p1, p2, ... ) => R
```

is exactly equivalent to the object type literal

```typescript
{ < T1, T2, ... > ( p1, p2, ... ) : R }
```

* To differentiate function types defined with object literal, check to see whether the object literal/interface contains a call signature

```typescript
interface A { // an object with a property called `log` which is function type
  log(num: number): number; // a property
}

let obj: A = {
  log(n) {
    return n;
  }
}

interface A1 { // a function type.
  (num: number): number // call signature
}

let func: A1 = (n: number) => n
```

## Optional parameters

Optional property can be marked with `?` following the parameter name. E.g.

```typescript
function parseInt(s: string, radix?: number): number {
  // ...
}

let result1 = parseInt('3'); // ok 
let result2 = parseInt('3', 2);  // ok
let result3 = parseInt('3', 2 , 'too many'); // ok
```

## Overloads

```typescript
let suits = ["hearts", "spades", "clubs", "diamonds"];

function pickCard(x: {suit: string; card: number; }[]): number;
function pickCard(x: number): {suit: string; card: number; };
function pickCard(x): any {
  if (typeof x == "object") {
    let pickedCard = Math.floor(Math.random() * x.length);
    return pickedCard;
  }
  else if (typeof x == "number") {
    let pickedSuit = Math.floor(x / 13);
    return { suit: suits[pickedSuit], card: x % 13 };
  }
}

let myDeck = [{
  suit: "diamonds", card: 2
}, {
  suit: "spades", card: 10
}, {
  suit: "hearts", card: 4
}];

let pickedCard1 = myDeck[pickCard(myDeck)];
alert("card: " + pickedCard1.card + " of " + pickedCard1.suit);

let pickedCard2 = pickCard(15);
alert("card: " + pickedCard2.card + " of " + pickedCard2.suit);
```

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.
