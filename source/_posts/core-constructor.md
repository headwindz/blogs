---
title: [MooTools源码分析-Core-构造函数的扩展]
date: 2017-05-19
tags: [source-code, frontend]
---

## 构造函数本质就是函数。
```javascript
typeof Array === 'function'
```

## 原型链的本质就是实例化对象上有个原型(\_\_proto\_\_属性)指向了构造函数的prototype属性
```javascript
function A() {} //A是构造函数
A.prototype.sayHi = function() {console.log('hi')};
var a = new A() //a是实例化对象
a.sayHi();
```
当我们访问a.sayHi的时候会经历
a.sayHi(自身属性) 
->
a.\_\_proto\_\_.sayHi(也就是A.prototype.sayHi) 
-> 
a.\_\_proto\_\_.\_\_proto\_\_.sayHi (因为a.\_\_proto\_\_指向A.prototype，所以a.\_\_proto\_\_.\_\_proto\_\_.sayHi等价于A.prototype.\_\_proto\_\_.sayHi,又因为A.prototype是一个对象，所以A.prototype是Object的一个实例，所以A.prototype.\_\_proto\_\_指向了Object.prototype, 所以A.prototype.\_\_proto\_\_等价于Object.prototype, 所以a.\_\_proto\_\_.\_\_proto\_\_.sayHi等价于Object.prototype.sayHi) 
->
a.\_\_proto\_\_.\_\_proto\_\_.\_\_proto\_\_.sayHi
(跟上面的推理一样，a.\_\_proto\_\_.\_\_proto\_\_.\_\_proto\_\_实际上是Object.prototype的原型，也就是Object.prototype.\_\_proto\_\_, 此为null，所以已经抵达原型链顶端，查询结束)

```javascript
function add(a, b) {return a+b;}
add instanceof Function === true; //函数add是内置构造函数Function的一个实例化对象
add.apply(null, [1,2]) === 3; // add.apply哪来的？
// add是Function的实例化对象，所以按照add的原型指向了Function.prototype所以就会有Function.prototype里定义的方法
add.apply === Function.prototype.apply 
typeof add.hasOwnProperty === 'function' // add.hasOwnProperty哪里来的？
```
<!-- more -->
## 静态方法跟实例化方法
静态方法是由构造函数进行调用的。实例化方法由实例化对象进行调用(本质是在实例化对象的原型上)
```javascript
var obj = {a: 'b'};
//keys是Object这个构造函数的一个静态方法。
Object.keys(obj); // ['a']
//hasOwnProperty定义在Object.prototype, 所以obj这个实例化对象的原型obj.\_\_proto\_\_(指向Object.prototype)会有这个方法
obj.hasOwnProperty('a');
```

因为静态方法实质是构造函数的一个属性，所以实例化并不会有这个方法。实例化对象只有定义在构造函数的prototype属性下的方法。
```javascript
obj.keys === undefined
```

## MoolTools源码
```javascript
Function.prototype.extend = function(key, value){
    this[key] = value;
}.overloadSetter();

Function.prototype.implement = function(key, value){
    this.prototype[key] = value;
}.overloadSetter();
```

前面提到函数都是Function的实例化对象，常见的构造函数例如Date, Array, Object等都是Function的实例化对象。当我们在Function.prototype下添加方法时，这些方法基于原型链里的原理都可被这些构造函数访问到。于是有了上面的代码之后，我们就可以用Array.extend, Array.implement等对构造函数进行扩展（当然扩展原生的构造函数是否是best practice值得商榷)。抛开overloadSetter我们先看下:

```javascript
Function.prototype.extend = function(key, value){
    //此处的this既为调用的构造函数。例如Array.extend()，那this就是Array
    //直接添加到this(构造函数)中。静态方法
    this[key] = value; 
};

Function.prototype.implement = function(key, value){
    //此处的this既为调用的构造函数。例如Array.extend()，那this就是Array
    //直接添加到prototype中。实例化对象可使用
    this.prototype[key] = value;
};

// example
Array.extend('staticPrint', function(arr){
    console.log('static print', arr);
});

Array.implement('instancePrint', function(){
   console.log('instancePrint', this); 
});

var ar = [1,2,3];
Array.staticPrint(ar);
ar.instancePrint();
```

当我们想要当时添加多个方法的时候，我们可以进行多次调用。例如：
```javascript
Array.implement('a', function() {console.log('a')})
Array.implement('b', function() {console.log('b')})
```
当然这样就略选笨拙，而且也违反了DRY(dont repeat yourself)原则。假如我们可以传入对象同时添加多个属性就好了。例如:
```javascript
Array.implement(
    {
        'a': function() {
            console.log('a')
        },
        'b': function() {
            console.log('b')
        }
    });
```
所以就有了overloadSetter。我们看一下overloadSetter的定义:
```javascript
Function.prototype.overloadSetter = function(usePlural){
    var self = this;
    return function(a, b){
        if (a == null) return this;
        if (usePlural || typeof a != 'string'){
            for (var k in a) self.call(this, k, a[k]);
            /*<ltIE8>*/
            forEachObjectEnumberableKey(a, self, this);
            /*</ltIE8>*/
        } else {
            self.call(this, a, b);
        }
        return this;
    };
};
```
overloadSetter定义在Function.prototype上，所以所有的函数实例都可以调用overloadSetter。这里要注意方法内部this的指向。this关键字为函数执行时的上下文，与定语时的语境无关。结合调用情况我们可以比较清晰的认识代码。假设我们的调用是:

```javascript
Array.implement(
    {
        'a': function() {
            console.log('a')
        },
        'b': function() {
            console.log('b')
        }
    });
```

那么对于这个调用，this值为：
```javascript
//定义
Function.prototype.implement = function(key, value){
    this.prototype[key] = value;
}.overloadSetter();

Function.prototype.overloadSetter = function(usePlural){
    //function(key, value){this.prototype[key] = value;}.overloadSetter()
    //所以此处的this,也就是self值为: function(key, value){this.prototype[key] = value;}
    var self = this;
    //这里返回的函数即为Array.implement。
    return function(a, b){
        //因为是Array.implement进行调用，所以此函数体里的this值为Array
        if (a == null) return this;
        if (usePlural || typeof a != 'string'){
            //此处遍历穿进来的参数对象，通过call进行逐个赋值
            // self.call(this,k, a[k])相对于Array.implement(k, a[k])
            for (var k in a) self.call(this, k, a[k]);
            /*<ltIE8>*/
            forEachObjectEnumberableKey(a, self, this);
            /*</ltIE8>*/
        } else {
            self.call(this, a, b);
        }
        return this;
    };
};
```
经过上面的扩展之后，我们可以：
```javascript
var arr = [1,2,3];
arr.a(); // 'a'
arr.b(); // 'b'
```