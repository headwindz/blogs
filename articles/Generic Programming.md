> Generic programming centers around the idea of abstracting from concrete, efficient algorithms to obtain generic algorithms that can be combined with different data representations to produce a wide variety of useful software.  

> — Musser, David R.; Stepanov, Alexander A., Generic Programming

## Goal

* To make the algorithm/method/solution work generically by encapsulating the details about data structures and operations. 
* To allow developers to focus on the solution instead of differentiating data structures in the implementation.

## Don't talk, show the example

### Calculate the sum of an array

```javascript
function sum(arr) {
  let total = 0;
  for(let i = 0; i < arr.length; i++) {
    total += arr[i];
  }
  return total; 
}

console.log(sum([1,2,3])); //6
```

We iterate through the array and add up each element to get the sum of the array. But what if we want a different operation, such as finding the max number:

### Find the max number in an array

```javascript
function max(arr) {
  let result = arr[0];
  for(let i = 1; i < arr.length; i++) {
    if(arr[i] > result) {
      result = arr[i]
    }
  }
  return max; 
}

console.log(max([1,2,3])); //3
```

### How can we do better?

There might be many cases in which we need to go through the array and do something with the elements in the array. It should be a good option to encapsulate the operation and leave it to the caller to determine what to do with each element. That's what [reduce](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce) is designed for. Let's implement our own **_reduce**.

```javascript

function _reduce(op, initialValue) {
  return (arr) => {
    let result;
    let index = 0;
    if(initialValue != null) {
      result = initialValue;
    } else {
      result = arr[0];
      index = 1;
    }
    for(let i = index; i < arr.length; i++) {
      result = op(result, arr[i])
    }
    return result;
  }
}
```

So now we can rewrite **sum** and **max** as:

```javascript
let sumWithReduce = _reduce((sum, item) => sum + item)
let maxWithReduce = _reduce((max, item) => max > item ? max : item)

console.log(sumWithReduce([1,2,3])); //6
console.log(maxWithReduce([1,2,3])); //3
```

### How can we do even better?

In the **_reduce** implementation above, we have abstracted away the operation detail and only keep the looping logic in the implementation. However, the implementation still relies on **for** and **array index** so it only works for **arrays**. We make it more generic with [Iterators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Iterators_and_Generators).

```javascript
function _genericReduce(op, initialValue) {
  return (iterables) => {
    let inited, result;
    if(initialValue != null) {
      result = initialValue;
      inited = true;
    }
    for(let item of iterables) {
      if(!inited) {
        result = item;
        inited = true;
      } else {
        result = op(result, item)
      }
    }
    return result;
  }
}

let sumWithGenericReduce = _genericReduce((sum, item) => sum + item)
let maxWithGenericReduce = _genericReduce((max, item) => max > item ? max : item)

console.log(sumWithGenericReduce([1,2,3])); //6
console.log(maxWithGenericReduce([1,2,3])); //3
```

It now works for all data structures that are *iterable*. Here is a reading note I wrote about [iterator](https://github.com/n0ruSh/the-art-of-reading/issues/10) if you are interested in.


```javascript
class Group {
  constructor(id) {
    this.id = id;
    this.members = [];
  }
  
  addMember(person) {
    this.members.push(person);
  }

  [Symbol.iterator]() {
    let index = 0;
    return {
      next: () => ({
        value: this.members[index++],
        done: index > this.members.length
      })
    };
  }
}

class Member {
  constructor(name, salary) { 
    this.name = name;
    this.salary = salary;
  }
}


## try to find the lowest salary in the group

let group = new Group('007');
group.addMember(new Member('mike', 1000))
group.addMember(new Member('john', 2000))
group.addMember(new Member('alfred', 3000))
let lowestSalary = _genericReduce((result, member) => {
  if(member.salary < result) {
    return member.salary
  }
  return result;
},  Number.MAX_SAFE_INTEGER)(group)
console.log(lowestSalary);
```

## Resource

[Code Sample](https://codepen.io/n0rush/pen/MzmzEJ)

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.





