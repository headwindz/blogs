---
title: 教练我要写单元测试
date: 2016-10-30
tags: [unit testing, backend, mocha, should]
---

## 为什么要写单元测试

+ 添加新功能或者重构时保证现有功能是可用的
+ 测试是最好的文档
+ 测试降低前后端交流成本(减少联调时出现的bugs)
+ 测试促使开发人员思考程序设计
+ 测试很有趣
+ 。。。。。(此处省略1万行)

<!-- more -->

## 怎么用Mocha(摩卡)写测试

首先先安装下Mocha

```bash
npm install mocha -g
```

### 同步方法的测试

假设我们有一个trim的方法.

```javascript
// trim.js
'use strict';

/**
 * Trim whitespace from the beginning and end of a string
 * @param {String} str - string to be trimed
 * @returns {String}
 */
function trim(str) {
    if (typeof str !== 'string') {
        throw new Error(`str is not string`);
    }
    return str.replace(/^\s+|\s+$/g, '');
}

module.exports = trim;
```

我们可以写一个test.js来测试trim方法.

```javascript
'use strict';

let trim = require('trim'); //引入上面的trim.js
let should = require('should'); //引入should断言库，用来判断源码的实际执行结果与预期结果是否一致

describe('string trim', () => {
    it('trim string with no spaces at beginning and end', () => {
        trim('hello world').should.be.eql('hello world');
    });

    it('trim string with space(s) at beginning', () => {
        trim('  hello world').should.be.eql('hello world');
    });

    it('trim string with tab(s) at beginning', () => {
        trim('\thello world').should.be.eql('hello world');
    });

    it('trim string with new line(s) at beginning', () => {
        trim('\nhello world').should.be.eql('hello world');
    });

    it('trim string with space(s) at beginning', () => {
        trim('hello world  ').should.be.eql('hello world');
    });

    it('trim string with tab(s) at beginning', () => {
        trim('hello world\t').should.be.eql('hello world');
    });

    it('trim string with new line(s) at beginning', () => {
        trim('hello world\n').should.be.eql('hello world');
    });

    it('trim string with spaces/tabs/new lines at beginning and end', () => {
        trim(' \t\nhello world \t\n').should.be.eql('hello world');
    });
});
```

然后通过运行以下命令即可对trim方法进行测试：

```bash
mocha test.js
```

结果:
```bash
string trim
    ✓ trim string with no spaces at beginning and end
    ✓ trim string with space(s) at beginning
    ✓ trim string with tab(s) at beginning
    ✓ trim string with new line(s) at beginning
    ✓ trim string with space(s) at beginning
    ✓ trim string with tab(s) at beginning
    ✓ trim string with new line(s) at beginning
    ✓ trim string with spaces/tabs/new lines at beginning and end

8 passing (12ms)
```

### 异步方法的测试

异步方法的测试只需要在你的测试结束时调用回调函数即可。通过给it()添加回调函数可以告知Mocha需要等待异步测试结束。

```javascript
'use strict';

let should = require('should');

describe('Async', function(){
    it("Async successful with done", function(done){
        setTimeout(function() {
            done();
        }, 200);
    });

    //如果要标志异步测试失败的话，可以通过给回调函数传递一个Error实例或者字符串
    it("Async failure with done", function(done){
        setTimeout(function() {
            done(new Error("This is a sample failing async test"));
        }, 200);
    });

    //也可以直接返回promise
    it("Async without done", function() {
        let testPromise = new Promise(function(resolve, reject) {
            setTimeout(function() {
                resolve("Hello!");
            }, 200);
        });

        return testPromise.then(function(result) {
            return result.should.be.equal("Hello!");
        });
    });
});
```

运行改解释脚本结果为:

```bash
Async
    ✓ Async successful with done (206ms)
    1) Async failure with done
    ✓ Async without done (209ms)


  2 passing (629ms)
  1 failing

  1) Async Async failure with done:
     Error: This is a sample failing async test
      at null._onTimeout (test.js:14:50)
```

## 怎么用istanbul测试覆盖率

首先先安装下istanbul

```bash
npm install istanbul -g
```

然后用istanbul跑测试文件.

```bash
istanbul cover _mocha test.js
```

运行结果如下：

```bash
string trim
    ✓ trim string with no spaces at beginning and end
    ✓ trim string with space(s) at beginning
    ✓ trim string with tab(s) at beginning
    ✓ trim string with new line(s) at beginning
    ✓ trim string with space(s) at beginning
    ✓ trim string with tab(s) at beginning
    ✓ trim string with new line(s) at beginning
    ✓ trim string with spaces/tabs/new lines at beginning and end

8 passing (10ms)

=============================================================================
Writing coverage object [/path/to/test/coverage/dir/coverage.json]
Writing coverage reports at [/path/to/test/coverage/dir/coverage]
=============================================================================

=============================== Coverage summary ===============================
Statements   : 100% ( 18/18 )
Branches     : 100% ( 0/0 )
Functions    : 100% ( 0/0 )
Lines        : 100% ( 18/18 )
================================================================================
```

这条命令同时还生成了一个 coverage 子目录，其中的 coverage.json 文件包含覆盖率的原始数据，coverage/lcov-report 是可以在浏览器打开的覆盖率报告，其中有详细信息，到底哪些代码没有覆盖到。


## 动态测试

```javascript
'use strict';

let mocha = new (require('mocha'))();
mocha.addFile('trim.js');

mocha.run(function(err) {
    process.on('exit', function() {
        process.exit(err);
    });
});
```