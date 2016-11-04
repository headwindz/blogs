---
title: 对象属性
date: 2016-10-23
tags: [javascript, frontend]
---

```javascript
'use strict';
let student = {
    name: 'michael'
};

Object.defineProperty(
    student,
    'age',
    {
        enumerable: false,
        value: 18
    }
);

Object.defineProperties(student,
    {
        grade: {
            value: 6,
            enumerable: true
        },
        sex: {
            value: 'male',
            enumerable: false
        }
    });

Object.prototype.x = 'inherited';

```

上面的代码创建了一个对象实例*student*。*student*拥有如下自身自身属性：

1. *name*: 可枚举
2. *age*: 不可枚举
3. *grade*: 可枚举
4. *sex*: 不可枚举

以及一个来自原型链上的自定义属性:

5. *x*: 可枚举

<!-- more -->
### 遍历对象属性方法1: for in循环遍历**可枚举**属性(**包括原型链**)

```javascript
for (var prop in student) {
    console.log(prop); //'name', 'grade', 'x'
}

//用hasOwnProperty可过滤出自身属性
for (var prop in student) {
    if (student.hasOwnProperty(prop)){
        console.log(prop); // 'name', 'grade'
    }
}
```

### 遍历对象属性方法2: Object.keys(o)遍历自身**可枚举**属性(i.e. **不包括原型链**)

```javascript
console.log(Object.keys(student));  // [‘name’, 'grade']

//验证是否为空对象可用:
Object.keys(student).length === 0;

Object.keys({}).length === 0;
```

### 遍历对象属性方法3: Object.getOwnPropertyNames(o)遍历**自身属性**(i.e. 不包括原型链, 包括自身不可枚举属性)

```javascript
console.log(Object.getOwnPropertyNames(student));  // [‘name’, ‘age’, 'grade', 'sex']
```

### 相关资料：

[原码链接](https://github.com/mzjh/web-learning/blob/master/blog/object-properties.js)
