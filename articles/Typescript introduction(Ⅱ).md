## Interfaces

* Typescript interfaces declare the structure of variables.
* Interface declarations produce `types`, not `values`.
* Interfaces provide the ability to name and parameterize object types. To represent primitive type, use `type` declaration

```typescript
type numOrString = number | string; // union of primitive type
type size = 'large' | 'small' | 'medium'; // union of string literals 
```

### Basic Interface

```typescript
interface WithName {
  name: string;
}

function printName(withNameObj: WithName) {
  console.log(withNameObj.name);
}
```

### Type checking rule

To check whether variable `y` can be assigned to variable/parameter `x`, the compiler checks each property of `x` to find a corresponding compatible property in `y`.

* The same rule for assignment is used when checking function call arguments.

* Object literals get `special` treatment and undergo excess property checking when assigning them to other variables, or passing them as arguments

[Example](https://www.typescriptlang.org/play/index.html#src=interface%20WithName%20%7B%0D%0A%20%20name%3A%20string%3B%0D%0A%7D%0D%0A%0D%0Afunction%20printName(withNameObj%3A%20WithName)%20%7B%0D%0A%20%20console.log(withNameObj.name)%3B%0D%0A%7D%0D%0A%0D%0Alet%20myObj%20%3D%20%7Bname%3A%20'mike'%2C%20age%3A%2018%7D%3B%0D%0A%2F%2F%20myObj's%20inferred%20type%20is%20%7Bname%3A%20string%2C%20age%3A%20number%7D%3B%0D%0AprintName(myObj)%3B%20%2F%2F%20OK%0D%0A%0D%0AprintName(%7Bname%3A%20'mike'%2C%20age%3A%2018%7D)%3B%20%2F%2F%20Error%2C%20excess%20property%20checking%20for%20object%20literal%0D%0AprintName(%7Bname%3A%20'mike'%2C%20age%3A%2018%7D%20as%20WithName)%3B%20%2F%2F%20type%20assertion%20as%20fix%0D%0A%0D%0Alet%20myObj1%3A%20WithName%20%3D%20%7Bname%3A%20'mike'%2C%20age%3A%2018%7D%3B%20%2F%2F%20error%2C%20excess%20property%20checking%20for%20object%20literal%0D%0Alet%20myObj2%3A%20WithName%20%3D%20myObj%3B%20%2F%2F%20OK) 

```typescript

let myObj = {name: 'mike', age: 18};
// myObj's inferred type is {name: string, age: number};
printName(myObj); // OK

// Error, excess property checking for object literal
printName({name: 'mike', age: 18});
// type assertion as fix
printName({name: 'mike', age: 18} as WithName);

// error, excess property checking for object literal
let myObj1: WithName = {name: 'mike', age: 18};

let myObj2: WithName = myObj; // OK
```

### Optional property

Optional property can be marked with `?` following the property name. E.g.

```typescript
interface SquareConfig {
    color?: string;
    width?: number;
}

function createSquare(config: SquareConfig): {color: string; area: number} {
    let newSquare = {color: "white", area: 100};
    if (config.color) {
        newSquare.color = config.color;
    }
    if (config.width) {
        newSquare.area = config.width * config.width;
    }
    return newSquare;
}

let mySquare = createSquare({color: "black"});
```

### Readonly properties

Readonly properties can be mark with `readonly` modifier. [E.g.](https://www.typescriptlang.org/play/index.html#src=interface%20Person%20%7B%0D%0A%20%20%20%20readonly%20identityId%3A%20number%3B%0D%0A%20%20%20%20readonly%20bloodType%3A%20'A'%20%7C%20'B'%3B%0D%0A%7D%0D%0A%0D%0Alet%20p1%3A%20Person%20%3D%20%7B%20identityId%3A%2010000%2C%20bloodType%3A%20'A'%20%7D%3B%0D%0Ap1.identityId%20%3D%20100005%3B%20%2F%2F%20error)

```typescript
interface Person {
    readonly identityId: number;
    readonly bloodType: 'A' | 'B';
}

let p1: Person = { identityId: 10000, bloodType: 'A' };
p1.identityId = 100005; // error!
```

## Classes

```typescript
class Point {
  constructor(public x: number, public y: number) {
    this.x = x;
    this.y = y;
  }
  public length() {
    return Math.sqrt(this.x * this.x + this.y * this.y);
  }
  static origin = new Point(0, 0);
}
```

## `value` and `type`

When you declare a class in TypeScript, you are actually creating multiple declarations at the same time.

* a value - constructor
* a type - the `type` of the instance of the class.

>  The following example introduces both a named type called `Point` (the class type) and a named value called `Point` (the constructor function) in the containing declaration space.

* The named type `Point` is exactly equivalent to

```typescript
interface Point {
  x: number;
  y: number;
  length(): number;
}
```

* The named value `Point` is a constructor function whose type corresponds to the declaration

```
let Point: {
  new(x: number, y: number): Point;
  origin: Point;
};
```

> The context in which a class is referenced distinguishes between the class type and the constructor function.

For example, in the assignment statement

```typescript
let p: Point = new Point(10, 20);
```

the identifier `Point` in the type annotation refers to the class type, whereas the identifier `Point` in the new expression refers to the constructor function object.

## `typeof` operator

The `typeof` operator takes an operand of any type and produces a value of the String primitive type. In positions where a type is expected, `typeof` can also be used in a type query to produce the type of an expression, in which case it should be followed by a `value`. 

```
let x = 5;
let y = typeof x;  // Use in an expression, equivalent to ` let y = 'number' `
let z: typeof x;   // Use in a type query, equivalent to ` let z: number `

let obj = { a: 3, b: 's' };
let another1: typeof obj = { a: 4 }; // error, property 'b' is missing
```

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.
