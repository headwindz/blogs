---
title: 同-异步
date: 2016-09-25
tags: [javascript, frontend]
---

## 假设我们有如下代码结构

|- a.txt     // 此文件内容为 {"nextFileToRead": "b.txt"}
|- b.txt     // 此文件内容为 "I am noob!"
|- main.js   // 为下面内容的js

## 同步

```javascript
let fs = require('fs');
let contentSync = fs.readFileSync('a.txt', 'utf-8');
console.log('hello world');
console.log(contentSync);

================================================
运行结果：
hello world
{'nextFileToRead': 'b.txt'}
```

运行顺序和代码顺序一致，由上至下。

<!-- more -->
## 异步

```javascript
let fs = require('fs');
let contentAsync = fs.readFile('a.txt', 'utf-8', (err, data) => {
    console.log('callback ', data);
});
console.log('hello world');
console.log(contentAsync);

================================================
运行结果：
hello world
undefined
callback  {"nextFileToRead": "b.txt"}
```
这里readFile方法为异步执行。并未阻塞下面的代码执行(下面2个console.log执行于readFile的callback调用之前)。当文件读取结束的时候，callback方法被调用打印出了文件内容。

## 异步的解决方案

### callback/回调
```javascript
let fs = require('fs');
fs.readFile('a.txt', 'utf-8', (err, dataA) => {
    console.log('data for a.txt: ', dataA);
    var jsonDataA = JSON.parse(dataA);
    fs.readFile(jsonDataA.nextFileToRead, 'utf-8', (err, data) => {
        console.log('data for next file: ', data);
    });
});
================================================
运行结果：
data for a.txt:  {"nextFileToRead": "b.txt"}
data for next file:  I am noob!
```

### Promise

用上面的回调容易造成多层嵌套回调。例如:

```javascript
fs.readFile('a.txt', 'utf-8', (err, dataA) => {
    fs.readFile(JSON.parse(dataA).nextFileToRead, 'utf-8', (err, dataB) => {
        fs.readFile(JSON.parse(dataB).nextFileToRead), (err, dataC) => {
            //继...续...嵌...套...
        });
    });
});
```

ES6引入的Promise的概念可以用来解决回调的嵌套问题。

```javascript
let fs = require('fs');
let promiseReadFile = (file) => {
    return new Promise((resolve, reject) => {
        fs.readFile(file, 'utf-8', (err, data) => {
            if (err) {
                reject(err);
            } else {
                resolve(data);
            }
        });
    });
};

promiseReadFile('a.txt')
.then((dataA) => {
    console.log('data for a.txt: ', dataA);
    let jsonDataA = JSON.parse(dataA);
    return promiseReadFile(jsonDataA.nextFileToRead);
}).then((dataB) => {
    console.log('data for next file: ', dataB);
});
================================================
运行结果：
data for a.txt:  {"nextFileToRead": "b.txt"}
data for next file:  I am noob!
```
嵌套回调不见了。then都在同一层级！

### Coroutine/协程
ES6的Generator/生成器可以用来实现协程. Generator的最大特点是可以使用yield关键字暂停携程的执行来达到交出协程的执行权，等到异步结果返回后将异步结果和执行权通过next()方法转回给协程.

```javascript
function* mainCoroutine() {
    let dataA = yield promiseReadFile('a.txt');
    console.log(dataA);
    let jsonDataA = JSON.parse(dataA);
    let dataB = yield promiseReadFile(jsonDataA.nextFileToRead);
    console.log(dataB);
}

let gen = mainCoroutine();
let phaseOne = gen.next(); //遇到yield语句，控制权移交给协程promiseReadFile
console.log(phaseOne);
phaseOne.value.then((dataA) => {
    let phaseTwo = gen.next(dataA); // 异步结束，控制权移交回协程mainCoroutine.
    // 协程coroutine遇到第2个yield, 控制权移交给协程promiseReadFile
    console.log(phaseTwo);
    phaseTwo.value.then((dataB) => {
        gen.next(dataB);
    });
});

================================================
运行结果：
{ value: Promise { <pending> }, done: false }
{"nextFileToRead": "b.txt"}
{ value: Promise { <pending> }, done: false }
I am noob!
```

那么问题来了。上面的例子我们写死了2个子协程移交控制权给协程。这样首先造成了代码的重复(DRY守则)，其次代码不够灵活。我们可以写个自动处理器来处理这类情况。

```javascript
let autoRunWithPromise = (genFn) => {
    let gen = genFn();

    let next = (ctl) => {
        let result = gen.next(ctl);
        if (result.done) {
            return result.value;
        } else {
            let promise = result.value;
            promise.then((data) => {
                next(data);
            });
        }
    }
    next();
}

function* mainCoroutineAuto() {
    let dataA = yield promiseReadFile('a.txt');
    console.log(dataA);
    let jsonDataA = JSON.parse(dataA);
    let dataB = yield promiseReadFile(jsonDataA.nextFileToRead);
    console.log(dataB);
}

autoRunWithPromise(mainCoroutineAuto);
```

上面是TJ大神的co的简陋版实现。co已经包装好了这么一个功能并且支持了:

    -promises
    -thunks (functions)
    -array (parallel execution)
    -objects (parallel execution)
    -generators (delegation)
    -generator functions (delegation)

#### thunks

最近项目里用到了thunkify-wrap所以对thunks做了些了解。感觉又涨了好多姿势！JS Thunk函数用来替换将多参数函数替换为只接受回调函数作为参数的单参数的版本。

```javascript

let Thunk = function(fn) {
    return function() {
        let args = [].slice.call(arguments);
        return (callback) => {
            args.push(callback)
            fn.apply(this, args);
        };
    };
};

let readFileThunk = Thunk(fs.readFile);
let fn = readFileThunk('a.txt', 'utf-8');
fn((err, data) => {
    console.log(data);
});

================================================
运行结果：
{"nextFileToRead": "b.txt"}
```

callback和promise有异曲同工之妙。把callback参数从多参数函数里抽出来之后便可以利用callback来进行执行权的转移。

```javascript

let Thunk = function(fn) {
    return function() {
        let args = [].slice.call(arguments);
        return (callback) => {
            args.push(callback)
            fn.apply(this, args);
        };
    };
};

let readFileThunk = Thunk(fs.readFile);

let autoRunWithThunk = (genFn) => {
    let gen = genFn();

    let next = (ctl) => {
        let result = gen.next(ctl);
        if (result.done) {
            return result.value;
        } else {
            let thunk = result.value;
            thunk((err, data) => {
                next(data);
            });
        }
    }
    next();
}

function* mainCoroutineAuto() {
    let dataA = yield readFileThunk('a.txt', 'utf-8');
    let jsonDataA = JSON.parse(dataA);
    let dataB = yield readFileThunk(jsonDataA.nextFileToRead, 'utf-8');
    console.log(dataB);
}

autoRunWithThunk(mainCoroutineAuto);

```

## 相关资料
[nodejs fs](http://nodejs.cn/api/fs)
[Generator](http://es6.ruanyifeng.com/#docs/generator)
[TJ大神的co](https://github.com/tj/co)