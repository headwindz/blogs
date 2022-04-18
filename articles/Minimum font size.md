## Minimum font size 

Major browsers have a default minimum font size that they support.

* For Chrome, the minimum font size can be found and configured following the path

```
Settings > Appearance > Customize fonts > Minimum font size
```


* For Edge, the minimum font size can be found and configured following the path

```
Settings > Appearance > Fonts > Customize fonts > Minimum font size
```

* To compute the minimum font size the browser supports, with script:

```javascript
const div = document.createElement('div');
div.style.fontSize = '3px';
div.style.lineHeight = '1';
div.innerHTML = 'a';
document.body.appendChild(div);
const minimumFontSize = div.clientHeight;
```

## How to display font size smaller than the minimum one

Say the minimum font size is `12px` and we want the text to be `9px`, We can use [`transform`](https://developer.mozilla.org/en-US/docs/Web/CSS/transform)

```css
.text {
  font-size: 12px;
  transform: scale(0.0.75); // 9 / 12 = 0.75
  transform-origin: left; // depending on how the text is aligned
}
```

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe
