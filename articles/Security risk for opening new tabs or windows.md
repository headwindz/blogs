## Background

Today eslint reports an error when I introduce [eslint-plugin-react](https://www.npmjs.com/package/eslint-plugin-react)

```
error  Using target="_blank" without rel="noopener noreferrer" is a security risk: see https://mathiasbynens.github.io/rel-noopener  react/jsx-no-target-blank
```

## Why

Opening a new tab/window, either by hyperlinks (i.e <a> tag with target attribute set to `_blank`) or programmatically calling [window.open](https://developer.mozilla.org/en-US/docs/Web/API/Window/open), will grant the newly-opened tab/window access back to the originating tab/window via [window.opener](https://developer.mozilla.org/en-US/docs/Web/API/Window/opener). Therefore, the newly opened tab/window can then change the `window.opener.location` to redirect to the phishing page in the background. Or execute some JavaScript on the opener-page on your behalf.

## How to fix

* [hyperlink types](https://developer.mozilla.org/en-US/docs/Web/HTML/Link_types)

Add `rel="noopenner"` to outgoing links. E.g.

```
<a href="https://abc.com" target="_blank" rel="noopener">
```

* Specify `noopener` in [window features](https://developer.mozilla.org/en-US/docs/Web/API/Window/open#Window_features)

```javascript
window.open('https://abc.com', 'security', 'noopener');
```

* Reset `opener` property

Note: this technique is subject to [Same Origin Policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)

```javascript
let nw = window.open('https://abc.com', 'security');
nw.opener = null;
```

## Reference

* [About rel=noopener](https://mathiasbynens.github.io/rel-noopener/)
* [Target="_blank" - the most underestimated vulnerability ever](https://www.jitbit.com/alexblog/256-targetblank---the-most-underestimated-vulnerability-ever/)

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.
