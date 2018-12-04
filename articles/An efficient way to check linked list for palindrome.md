## Background

> A palindrome is a word, phrase, number or sequence of words that reads the same backwards as forwards.

E.g.  `aba` is a palindrome while `abc` is not.

## Q1. how to check whether a string is a palindrome

`String` in javascript works almost the same as `Array`. It wouldn't be trival to check for palindrome with the help of build-in `Array` utility support.

* With iteration

Idea: one pointer from the beginning and another pointer from the end, move both pointers towards the center and compare the values along the way

```javascript
function isPalindrome(str) {
  const len = str.length;
  for(let i = 0; i < len / 2; i++) {
    if (str[i] !== str[len - i - 1]) {
      return false;
    }
  }
  return true;
}

console.log(isPalindrome('a')); // true
console.log(isPalindrome('ab')); // false
console.log(isPalindrome('aba')); // true
console.log(isPalindrome('abcba')); // true
console.log(isPalindrome('abcda')); // false
console.log(isPalindrome('abccba')); // true
```

* With recursion

Similar to the solution above but implemented in recursive way.

```javascript
function isPalindromeRecursive(str) {
  const len = str.length;
  if (len === 0 || len === 1) {
    return true;
  }
  return str[0] === str[len - 1] && isPalindromeRecursive(str.slice(1, len - 1))
}


console.log(isPalindromeRecursive('a')); // true
console.log(isPalindromeRecursive('ab')); // false
console.log(isPalindromeRecursive('aba')); // true
console.log(isPalindromeRecursive('abcba')); // true
console.log(isPalindromeRecursive('abcda')); // false
console.log(isPalindromeRecursive('abccba')); // true
```

## What is the string is in a singly linked list format ?

> A linked list consists of nodes where each node contains a data field and a reference(link) to the next node in the list

![linkedlist](https://n0rush-blogs.oss-cn-beijing.aliyuncs.com/linkedlist.png)

With singly linked list, only the `head` node of the linked list is available and the only way to visit a specific node is by traversing from `head` with `next` pointer up to the node. The main point for checking palindrome is to find the node in the center. Basically this can be achieved by having two pointers go from `head`, one moves two steps forward and the other one step forward until the faster one reaches the end. Also, we build a previous link as the slower pointer moves towards the center. E.g.


list: A -> B -> C -> null/end

Step 1.  Both pointers at `A`
Step 2.  Slower pointer moves to `B`, build previous link (i.e. B.prev = A). Faster pointer moves to `B` then to `C`
Step 3.  Faster pointer is at then end (C.next == null). 
Step 4.  Center pointer is at B (i.e. where slow pointer is). `NOTE: where center is depends on the parity of the number of nodes`
Step 5.  Move slower pointer forward and center pointer backend, check the equality of the value. If value is not the same, not a palindrome.
Step 6.  Slow pointer successfully reaches the end => is a palindrome.

```javascript
class LinkedListNode {
  constructor(char, next) {
    this.value = char;
    this.next = next;
  }
}

class LinkedList {
  constructor(chars) {
    const len = chars.length;
    let currentNode = null;
    for (let i = len - 1; i >= 0; i--) {
      let node = new LinkedListNode(chars[i], currentNode);
      currentNode = node;
    }
    this.head = currentNode;
  }

  isPalindrome() {
    let center, slow, quick;
    center = slow = quick = this.head;
    if (slow.next == null) {
      return true;
    }
    while(quick.next != null) {
      slow = slow.next;
      slow.prev = center;
      center = slow
      quick = quick.next
      if (quick.next == null) {
        // even number
        center = slow.prev;
      } else {
        quick = quick.next;
      }
    }
    do {
      if(slow.value !== center.value) {
        return false;
      }
      slow = slow.next;
      center = center.prev;
    } while(slow != null) 
    return true;
  }
}

console.log(new LinkedList(['a']).isPalindrome()); // true
console.log(new LinkedList(['a', 'b']).isPalindrome()); // false
console.log(new LinkedList(['a', 'b', 'a']).isPalindrome()); // true
console.log(new LinkedList(['a', 'b', 'c', 'b', 'a']).isPalindrome()); // true
console.log(new LinkedList(['a', 'b', 'c', 'd', 'a']).isPalindrome()); // false
console.log(new LinkedList(['a','b','c', 'c', 'b', 'a']).isPalindrome()); // true

```

## Resource

[Code Sample](https://codepen.io/n0rush/pen/gQyJwK)

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.
