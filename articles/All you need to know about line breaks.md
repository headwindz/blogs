## What’s `downwards arrow with corner leftwards (↵)`

It’s a [unicode code character](http://www.fileformat.info/info/unicode/char/21B5/index.htm) with code point `8629`.

```javascript
let s = '↵';
let codePont = s.charCodeAt(0); // 8629
let hex = codePoint.toString(16); // 21b5
let hexFormat = '\u21b5'; // '↵'
```

> The character is a human-friendly representation of a newline in the console, like \n is in JavaScript   

```javascript
let arr = ['a\nb', 'a↵b'];
console.log(arr);
/*
["a↵b", “a↵b”]    // note they look the same on console
*/

console.log(arr[0]);
/*
"a
b"  // note it’s indeed a line break
*/

console.log(arr[1]);
/*
"c↵d" // note it’s indeed an arrow
*/

console.log(arr[0] === arr[1]); // false
```

`How the developer tools express a new line in debugging output and how they treat the same character in source code are two different things`

## How to preserve line break in html

> Whitespace in html is compressed into a single space by default

```javascript
document.write('Hello\nWorld');
// You will see 'Hello World' on the page
```

There are two straightforward solutions to make ‘\n’ work in html

### Convert \n to `br` tag

[<br>: The Line Break element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/br)

```javascript
document.write('Hello\nWorld'.replace(/\n/g, '<br/>'))
```

### `pre` tag

The [HTML pre tag](https://www.w3schools.com/tags/tag_pre.asp) defines reformatted text.

### css `white-space`

The [white-space](https://developer.mozilla.org/en-US/docs/Web/CSS/white-space) CSS proper sets how white space inside an element is handled. Possible values are:

* normal: Sequences of whitespace will collapse into a single whitespace. Text will wrap when necessary.
* nowrap: Sequences of whitespace will collapse into a single whitespace. Text will never wrap to the next line. The text continues on the same line until a <br> tag is encountered.
* pre: Whitespace is preserved by the browser. Text will only wrap on line breaks. Acts like the <pre> tag in HTML.
* pre-wrap: Whitespace is preserved by the browser. Text will wrap when necessary, and on line breaks.
