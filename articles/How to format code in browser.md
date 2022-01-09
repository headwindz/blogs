# Format code

There are times where we get codes by computation and would like to format them before displaying on the page. 

## [prettier](https://prettier.io/)

Prettier comes with cli support which can be used on `node` environment. However, using prettier to format code on browser is possible. More detail can be found [here](https://prettier.io/docs/en/browser.html). E.g.

```js
import prettier from "https://unpkg.com/prettier@2.5.1/esm/standalone.mjs";
import parserBabel from "https://unpkg.com/prettier@2.5.1/esm/parser-babel.mjs";
import parserHtml from "https://unpkg.com/prettier@2.5.1/esm/parser-html.mjs";

console.log(
  prettier.format("const html=/* HTML */ `<DIV> </DIV>`", {
    parser: "babel",
    plugins: [parserBabel, parserHtml],
  })
);
// Output: const html = /* HTML */ `<div></div>`;
```

[Sandbox example](https://codesandbox.io/s/code-format-gqi1h)

## [js-beautify](https://github.com/beautify-web/js-beautify)

```js
import { js_beautify } from "js-beautify";
const rawJsCode = `
function add(a) {
return function(b){
return a + b;
}
}

sum(3)(5)
`;

js_beautify(rawJsCode);

/* Output:
 * function add(a) {
 *    return function(b) {
 *        return a + b;
 *    }
 * }
 *
 * sum(3)(5)
```

[Sandbox example](https://codesandbox.io/s/code-format-gqi1h)

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.