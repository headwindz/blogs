---
title: 编(解)码与加密
date: 2016-10-31
tags: [frontend]
---

从客户端的角度来看，编码与加密都是把一个字符串按照一定的算法转换成另一个抽象/形式的字符串。编码是可逆(i.e. 可解码)，但是加密是不可逆的(i.e. 难解密)。

## Base64(编码)

Base64不是加密而是一种编码方案，很容易被解码。所以Base64不适合于用来保护敏感数据。例如，当我们在http请求中用Base64来编码用户名和密码时， 虽然可使这些信息变得生涩难懂，但是它们依然可以被轻松的解码获得。安全不是使用编码的主要目的。Base64编码可用于把HTTP不兼容的字符转化成HTTP可兼容的字符。浏览器里内置了atob和btoa方法可以用来编码和解码Base64。*** Base64只适用于ASCII***

```javascript
// Base64编码
window.btoa('hello world') //aGVsbG8gd29ybGQ=

// Base64解码
window.atob('aGVsbG8gd29ybGQ=') //hello world

// 非ASCII
window.btoa('✓') //报错
```

Base64还可以用可读字符来表示2进制数据。这种写法比一堆1和0更有效。

### Base64表示图片

例如：

```html
<!-- 在html里引入一张Data URI scheme定义的图片 -->
<img alt="Data URI Image" src="data:image/png;base64，4AAQSkZJRgABAQAAAQABAAD..." />
```

在上面的Data URI中，data表示取得数据的协定名称，image/png是数据类型名称，base64是数据的编码方法，逗号后面就是这个image/png文件base64编码后的数据。通过这种形式我们把图像文件的内容直接写在了HTML文件中，优点就是节省了一个Http请求。缺点就是不容易维护(当看那串字符串你能知道那是什么图片么！而且后期要改图片的话肿么办)，而且浏览器不会缓存这种图像。

<!-- more -->

## URI(编码)

encodeURI方法对URI进行完整的编码，因此对以下在URI中具有特殊含义的ASCII标点符号不会进行转义:
```bash
  ;/?:@&=+$，#
```

```javascript
  encodeURI('https://mzjh.github.io/hello world') // https://mzjh.github.io/hello world"
  encodeURI(';/?:@&=+$，#') // ;/?:@&=+$，#
  decodeURI('https://mzjh.github.io/hello%20world') //https://mzjh.github.io/hello world
```

如果URL组件中含有分隔符，则使用encodeURIComponent方法分别对各组件进行编码。

```javascript
  encodeURIComponent('https://mzjh.github.io/hello world') //https%3A%2F%2Fmzjh.github.io%2Fhello%20world
  encodeURIComponent(';/?:@&=+$，#') // %3B%2F%3F%3A%40%26%3D%2B%24%2C%23
  decodeURI('https%3A%2F%2Fmzjh.github.io%2Fhello%20world') //https%3A%2F%2Fmzjh.github.io%2Fhello%20world
```

经常可以看到使用encodeURIComponent对跳转URI参数进行编码。

```javascript
  let redirectUri = "https://mzjh.github.io"; //跳转URI参数
  redirectUri = encodeURIComponent(redirectUri);
  let url = `https://login/in/page.com?redirectTo=${redirectUri}`;
  console.log(url) // https://login/in/page.com?redirectTo=https%3A%2F%2Fmzjh.github.io
```

上面url里的redirectUri参数也是一个URL，如果不编码或者只使用encodeURI编码(i.e. /未进行编码)，则可能会影响整个URL的跳转。

## MD5(加密)

MD5是一种哈希函数，只能单/正向使用并且不能被解密。

```javascript
'use strict';

let crypto = require('crypto');
let md5 = function(content) {
    return crypto
            .createHash('md5')
            .update(content)
            .digest('hex');
};
console.log(md5('hello world')); //5eb63bbbe01eeed093cb22bb8f5acdc3
```

MD5经常用来对敏感信息进行加密保证信息的安全性。例如可以用来对用户的登录密码进行加密。由于加密只能正向使用所以当用户忘记密码的时候管理员也无法知道原始密码。这时只能给用户一个临时密码并让用户用临时密码进行密码重置。

## RSA(加密)

RSA是一种不对称加密算法。你会拥有2个秘钥：一个公钥(public key)和一个私钥(private key)。你可以用公钥来进行加密然后用私钥来进行解密。

常见RSA使用场景：
  - Github/Gitlab SSH
  - [登录远程服务器 SSH](https://mzjh.github.io/2016/09/29/ssh)
  - [Https证书](https://mzjh.github.io/2016/10/21/nginx)


## 相关资料

[Base64图片转换工具](http://www.atool.org/img2base64.php)
