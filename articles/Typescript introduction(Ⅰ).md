## What is Typescript

> Typescript - Javascript that scales

## Why Typescript

### Static type system

TypeScript can do static type checking at compile time. Types are erased before emitting compiled Javascript, result in zero run-time overhead to program execution.

### Scalability

It's pretty useful for large-scale system and makes code refactoring easier and under control. E.g.

```javascript
// You have no idea what parameter the greet funciton accpets without peaking into the function implementation  
function greet(person) {
  if (person.sex === 'male') {
    return `Mr ${person.first} ${person.second}`
  } else if (person.sex === 'female') {
    return `Mis ${person.first} ${person.second}`
  }
  return `${person.first} ${person.second}`
}
```

compared to the Typescript version

```typescript
interface Person {
    sex?: string
    first: string
    second: string
}

function greet(person: Person): string {
    // ...
}
```

### IDE support - symbol based navigation + statement completion


## Starter

In Typescript, types can be associated with variables through explicit type annotations, such as

```tyepscript
let x: number;
```

or through implicit type inference, as in

```typescript
let x = 1;
```

### Types and Values

Typescript is a superset of Javascript. What works in Javascript should also work in Typescript. The addon from Typescript, as its name indicates, is `type`. In Typescript, there are `values` and `types`. TypeScript erases all `types` information before emiting JavaScript while `values` are preserved as in Javascript.

#### What results in `types`:

- A type alias declaration (type sn = number | string;)
- An interface declaration (interface I { x: number[]; })
- A class declaration (class C { })
- An enum declaration (enum E { A, B, C })
- An import declaration which refers to a type

#### What results in `values`:

- let, const, and var declarations
- A namespace or module declaration which contains a value
- An enum declaration
- A class declaration
- An import declaration which refers to a value
- A function declaration

## Basic Types

* Boolean

```typescript
let isChecked: boolean = false;
```

* Number

```typescript
let num: number = 6;
```

* String

```typescript
let color: string = "blue";
```

* Array

```typescript
let list: number[] = [1, 2, 3];

let list: Array<number> = [1, 2, 3];  // This doesnt work in JSX as `<` and `>` are used in element tag
```

* Tuple

```typescript
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ["hello", 10]; // OK
// Initialize it incorrectly
x = [10, "hello"]; // Error
```

* Enum

```typescript
enum Color {Red = 1, Green, Blue}
let c: Color = Color.Green;
```

> Note that `Enum` produce both a `value` and a `type`.

```
// compiled code
var Color;
(function (Color) {
    Color[Color["Red"] = 1] = "Red";
    Color[Color["Green"] = 2] = "Green";
    Color[Color["Blue"] = 3] = "Blue";
})(Color || (Color = {}));
var c = Color.Green;
```

* Any - try to avoid 

It's useful when migrate from Javascript to Typescript. 

* Void

`void` is a little like the opposite of `any`: the absence of having any type at all. You may commonly see this as the return type of functions that do not return a value:

```typescript
function warnUser(): void {
  console.log("This is my warning message");
}
```

* Object:

> Note: `object` is not the same as `Object`.

* `object` is a basic type that represents the non-primitive type, i.e. any thing that is not number, string, boolean, symbol, null, or undefined.
* `Object` is a built-in interface as shown below. It is almost any type.

```
interface Object {
    /** The initial value of Object.prototype.constructor is the standard built-in Object constructor. */
    constructor: Function;

    /** Returns a string representation of an object. */
    toString(): string;

    /** Returns a date converted to a string using the current locale. */
    toLocaleString(): string;

    /** Returns the primitive value of the specified object. */
    valueOf(): Object;

    /**
      * Determines whether an object has a property with the specified name.
      * @param v A property name.
      */
    hasOwnProperty(v: PropertyKey): boolean;

    /**
      * Determines whether an object exists in another object's prototype chain.
      * @param v Another object whose prototype chain is to be checked.
      */
    isPrototypeOf(v: Object): boolean;

    /**
      * Determines whether a specified property is enumerable.
      * @param v A property name.
      */
    propertyIsEnumerable(v: PropertyKey): boolean;
}
```

Check the [demo](https://www.typescriptlang.org/play/index.html#src=function%20logWithBasicObject(obj%3A%20object)%20%7B%0D%0A%20%20%20%20console.log(obj.hasOwnProperty)%3B%0D%0A%7D%0D%0A%0D%0AlogWithBasicObject(%7Ba%3A%20'b'%7D)%0D%0AlogWithBasicObject('ab')%0D%0A%0D%0A%0D%0Afunction%20logWithInterfaceObject(obj%3A%20Object)%20%7B%0D%0A%20%20%20%20console.log(obj.hasOwnProperty)%3B%0D%0A%7D%0D%0A%0D%0AlogWithInterfaceObject(%7Ba%3A%20'b'%7D)%0D%0AlogWithInterfaceObject('ab')) for difference: 

```
function logWithBasicObject(obj: object) {
    console.log(obj.hasOwnProperty);
}

logWithBasicObject({a: 'b'}); // OK
logWithBasicObject('ab'); // Error: Argument of type '"ab"' is not assignable to parameter of type 'object'

function logWithInterfaceObject(obj: Object) {
    console.log(obj.hasOwnProperty);
}

logWithInterfaceObject({a: 'b'}); // OK
logWithInterfaceObject('ab'); // OK
```

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.
