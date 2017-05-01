---
title: JS prototype的那些事
date: 2016-07-09
tags: [javascript, frontend]
---
## 什么是prototype

```javascript
// 代码块1
var obj = {};
console.log(obj.hasOwnProperty('hello')); //false
```

这里obj是一个空对象。为什么会有hasOwnProperty这个方法呢！一言不合先贴段代码。

```javascript
// 代码块2
var obj = {};
console.log(typeof obj.__proto__); //'object'
console.log(typeof Object); //'function'
console.log(obj.__proto__ === Object.prototype); //true
console.log(Object.prototype.hasOwnProperty('hasOwnProperty')); //true
```
obj其实并不是真正意味上的空对象。obj有个\_\_proto\_\_属性指向了Object(构造器)的prototype属性。

有坑出没：
1. 构造器的prototype属性 (i.e. 上例中的Object.prototype)
2. 实例对象的原型
3. 实例对象的\_\_proto\_\_ 属性(i.e. obj.\_\_proto\_\_)

2是一个概念，它的值是3。 3的值等于1。

<!-- more -->
## prototype chain

那么问题来了。就算obj这个实例对象的原型有hasOwnProperty这个方法。但是obj本身没有呀。为什么obj.hasOwnProperty没有报错。
这是因为当js执行obj.hasOwnProperty的时候会先去寻找obj.hasOwnProperty。如果obj.hasOwnProperty不存在的话，会查找下obj的原型, 也就是obj.\_\_proto\_\_.hasOwnProperty。然后层层往下找原型(即\_\_proto\_\_属性)直到找到或者原型已经底层(也就是null)为止.

## prototype有什么用

面向对象！！！

直接上代码了。

```javascript
// 代码块3
function Person(name, age) {
    this.name = name;
    this.age = age;
}

Person.prototype = {
    getName: function() {
       return this.name;
    },

    getAge: function() {
        return this.age;
    },

    gettingOld: function() {
       return this.age++;
    }
}

var mike = new Person('michael', 20);
console.log(mike.getName()); //'michael'
console.log("age this year: " + mike.getAge()); //'age this year: 20'
mike.gettingOld();
console.log(mike.getAge()); //21
```

看起来是不是很像Java/C++里面的class ^__^

## new 关键字

new关键字用来构建实例化对象主要做了3件事情
1. 新建一个空对象 {}
2. 赋予这个对象对应的prototype
3. 把这个空对象扔给构造器进行调用

假设现在我们把new当成一个方法来实现。它会接受一个构造器并返回相应生成的实例化对象。

```javascript
// 代码块4
function myNewImplementation(constructorFn) {
    var result = {};
    result.__proto__ = constructorFn.prototype; //实例化对象的原型(即__prototo__属性)为构造器的prototype属性
    var args = [].slice.call(arguments, 1); //第一个参数为构造器, 过滤掉
    constructorFn.apply(result, args); //调用构造器对实例化对象进行操作
    return result;
}
```

那么代码块3的

```javascript
var mike = new Person('michael', 20);
```

就等价于

```javascript
var mikeWithNewImplementation = myNewImplementation(Person, 'michael', 20);
```

## 继承

继承在js里是利用了原型链的原理实现的。假设我们现在要继承上面的Person。

```javascript
function Student(name, age, grade) {
    Person.call(this, name, age);
    this.grade = grade;
}

Student.prototype = (function() {
    //新建一个傀儡构造器
    function dummyCtr() {
    }
    //链接到Person构造器的prototype属性
    dummyCtr.prototype = Person.prototype;
    return new dummyCtr();

    // 或者
    // var f = {};
    // f.__proto__ = Person.prototype;
    // return f;
})();

// 给Student添加子类新方法
Student.prototype.getGrade = function() {
    return this.grade;
};

// 覆盖父类方法
Student.prototype.getAge = function() {
    return "子类调用";
};

var xiaomingtongxue = new Student('xiaoming', 6, 1);
console.log(xiaomingtongxue.getName()); //'xiaoming'
console.log(xiaomingtongxue.getAge()); //'子类调用'
console.log(xiaomingtongxue.getGrade()); //1
```

因为继承的普遍性所以JS的Object里专门有一个create方法用来实现原型的继承。

```javascript
Student.prototype = (function() {
    //新建一个傀儡构造器
    function dummyCtr() {
    }
    //链接到Person构造器的prototype属性
    dummyCtr.prototype = Person.prototype;
    return new dummyCtr();

    // 或者
    // var f = {};
    // f.__proto__ = Person.prototype;
    // return f;
})();

//等价于
Student.prototype = Object.create(Person.prototype);
```

综上。当你真正要实现一个真正意义上的空对象的时候，可以这么做。

```javascript

function NoPrototypeCtr () {}

NoPrototypeCtr.prototype = Object.create(null);

var realEmptyObj = new NoPrototypeCtr();
console.log(realEmptyObj.hasOwnProperty); //undefined. 此方法不存在,因为没有继承Object.prototype
```

## ES6的语法糖

ES6支持了class和继承的语法糖。让代码看起来跟面相对象(Java/C++等)语言更类似。 本质其实还是一样的依赖于原型

```javascript
class PersonES6 {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    getName() {
        return this.name;
    }

    getAge() {
        return this.age;
    }

    gettingOld() {
        return this.age++;
    }
}

class StudentES6 extends PersonES6 {
    constructor(name, age, grade) {
        super(name, age);
        this.grade = grade;
    }

    getGrade() {
        return this.grade;
    }

    getAge() {
        return "子类调用";
    }
}

var mikeES6 = new PersonES6('michael', 20);
console.log(mikeES6.getName()); // 'michael'
console.log("age this year: " + mikeES6.getAge()); //'age this year: 20'
mikeES6.gettingOld();
console.log(mikeES6.getAge()); //21
console.log(typeof PersonES6); //'function'
console.log(mikeES6.__proto__ === PersonES6.prototype); //true

var xiaomingtongxueES6 = new StudentES6('xiaoming', 6, 1);
console.log(xiaomingtongxueES6.getName()); //'xiaoming'
console.log(xiaomingtongxueES6.getAge()); // '子类调用''
console.log(xiaomingtongxueES6.getGrade()); //1
console.log(typeof StudentES6); //'function'
console.log(xiaomingtongxueES6.__proto__ === StudentES6.prototype); //true
console.log(PersonES6.prototype.isPrototypeOf(xiaomingtongxueES6)); //true
```