---
title: 那些年看过的奇技淫巧
date: 2016-09-20
tags: [javascript, frontend]
---
## 克隆一个数组

常见方法：

```javascript
var arr = [2, 4, 100];

// for循环！
var copy1 = [];
for (var i = 0, len = arr.length; i < len; i++) {
    copy1[i] = arr[i];
}

// forEach
var copy2 = [];
arr.forEach(function(ele) {
    copy2.push(ele);
});
```
奇技淫巧:

```javascript
var copy3 = [];
copy3.push.apply(copy3, arr);

//ES6 spread operator
copy3.push(...arr);

//ES6 destructuring
let [...copy3] = arr;

```
<!-- more -->

## 查找数组最小值

奇技淫巧:

```javascript
var arr = [2, 10, 4];
Math.min.apply(null, arr);
```


## 重复字符串

```javascript
let repeatStr = function(str, n) {
    return new Array(n + 1).join(str);
}

```

## 补全日期

奇技淫巧:

```javascript
// convert date to YYYY-MM-DD HH:MM:SS.MSS
'use strict';
let util = require('util');

let num = function(item,len){
    let d = (len || 2) - ('' + item).length;
    return new Array(d + 1).join('0') + item;
};

function dateToString(date) {
    let year = date.getFullYear();
    let month = date.getMonth() + 1;
    let da = date.getDate();
    let hour = date.getHours();
    let minute = date.getMinutes();
    let second = date.getSeconds();
    let milliSecond = date.getMilliseconds();

    return util.format('%s-%s-%s %s:%s:%s.%s',
            year,
            num(month),
            num(da),
            num(hour),
            num(minute),
            num(second),
            num(milliSecond,3));
}
```

## 支持数组和单个参数
```javascript
'use strict';

class Person {
    constructor(age) {
        this.age = age;
    }

    getOld() {
        this.age++;
        return this;
    }
}

let mike = new Person(18);
let rob = new Person(30);

let turnOld = function(persons) {
    if (!Array.isArray(persons)) {
        persons = [persons];
    }

    persons.forEach((person) => {
        person.getOld();
    });

    return persons;
}

console.log(turnOld(mike));
console.log(turnOld([mike, rob]));
```

## 只执行一次的函数
```javascript
'use strict';

let fn = function() {
    console.log('executed', this, arguments);
}

let runOnce = function(fn) {
    if (typeof fn !== 'function') {
        throw new Error('not a function');
    }
    return function() {
        if(!!fn) {
            fn.apply(this, arguments);
            fn = null;
        } else {
            console.log('function already executed');
        }
    }
}

```

## 正则解析url中的querys
```javascript
'use strict';

let querys = function(url) {
    let regExp = /([^?=&]+)(=([^&]*))?/g
    let result = {};
    if (!!url) {
        url.replace(regExp, (all, $1, $2, $3) => {
            console.log($1, $2);
            result[$1] = $3
        });
    }
    return result;
}

let url = 'http://mzjh.github.com/?hello=world&fish=cat&good=nice&hey';
console.dir(querys(url));
```

## 正则获取后缀
```javascript
/\.(\w+)$/
```
(持续更新中...)
