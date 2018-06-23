## Relevant basic concepts

### Property accessors

```javascript
let obj = {
  a: 'b'
};

// dot notation
console.log(obj.a); // 'b'
// bracket notation
console.log(obj['a']); // 'b'
// through prototype chain
console.log(obj.toString, obj.toString === Object.prototype.toString); // ƒ toString() { [native code] } true
```

### Check whether an property exists in object

* [in operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/in): returns true if the specified property is in the specified object *or its prototype chain*.
* [hasOwnProperty](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty): returns a boolean indicating whether the object has the specified property as its own property (as opposed to inheriting it).

```javascript
let obj = {
  a: 'b'
};

console.log('a' in obj); //true
console.log('toString' in obj); //true
console.log(obj.hasOwnProperty('a')); //true
console.log(obj.hasOwnProperty('toString')); //false
```

### [Object.defineProperty](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty) && [Object.defineProperties](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperties) - defines new or modifies existing properties directly on an object, returning the object.

```javascript 
let obj = {
  a: "b"
};

Object.defineProperties(obj, {
  c: {
    value: 'd'
  },
  e: {
    value: 'f'
  }
});
console.log(obj); // {a: "b", c: "d", e: "f"}
```

#### [getter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/get) and [setter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/set)

We can use getters and setters to generate computed property. E.g.

```
let people = {
  firstName: 'michael',
  lastName: 'zheng',
  get fullName() {
    return `${this.firstName} ${this.lastName}`
  },
  set fullName(val) {
    [this.firstName, this.lastName] = val.split(' ');
  }
}

console.log(people.firstName, people.lastName, people.fullName);
//"michael", "zheng", "michael zheng"
people.fullName = 'hello world';
console.log(people.firstName, people.lastName, people.fullName);
//"hello", "world", "hello world"
```

## There are three ways to iterate through objects

```javascript
let student = {
  name: "michael"
};

Object.defineProperties(student, {
  age: {
    enumerable: false,
    value: 18
  },
  grade: {
    value: 6,
    enumerable: true
  },
  sex: {
    value: "male",
    enumerable: false
  }
});

Object.prototype.x = "inherited";
```

In the sample above, we create an object *student*. *student* has following properties:

* _name_: self enumerable property
* _age_: self non-enumerable property
* _grade_: self enumerable property
* _sex_: self non-enumerable property

as well as a custom property from prototype chain: 

* _x_: enumerable

### [for...in](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in) iterates over *enumerable* properties of an object, **including prototype chain**.

```javascript
for (let prop in student) {
  console.log(prop); //'name', 'grade', 'x'
}

for (let prop in student) { // self properties only
  if (student.hasOwnProperty(prop)) {
    console.log(prop); // 'name', 'grade'
  }
}
```

### [Object.keys](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) returns an array of a given object's property names, only iterate through self enumerable properties.(i.e. **not including prototype chain**)

```javascript
console.log(Object.keys(student)); // [‘name’, 'grade']

//check whether is plain object:
Object.keys(student).length === 0; //false

Object.keys({}).length === 0; //true
```

### [Object.getOwnPropertyNames](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyNames) returns an array of all self properties (including non-enumerable properties) found directly upon a given object.

```javascript
// will not iterate through prototype chain
console.log(Object.getOwnPropertyNames(student)); // [‘name’, ‘age’, 'grade', 'sex']
```

### Summarize

| methods  | through prototype chain | enumerable only |
| :---: | :---: | :---: |
| for...in | Y | Y |
| Object.keys | N | Y |
| Object.getOwnPropertyNames | N | N | 


## Notice

* If you want to follow the latest news/articles for the series of reading notes, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.