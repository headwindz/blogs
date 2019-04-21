> Centering in CSS is should NOT make you frustrated.

# Horizontal

## [text-align](https://developer.mozilla.org/en-US/docs/Web/CSS/text-align)

> text-align property can be inherited

### Use Cases:

* Inline or inline-block element(s)

### [Demo](https://codepen.io/n0rush/pen/bJKoNV)

```html
<header>
  This text is centered.
</header>

<nav>
  <a>App</a>
  <a>Shop</a>
  <a>Login</a>
</nav>
```

```css
header, nav {
  text-align: center
}
```

### [Inheritance Demo](https://codepen.io/n0rush/pen/ZZRXGJ)

```html

<div class='container'>
  <div>
    <p> Hello to my demo </p>
  </div>
</div>
```

```css
.container {
  text-align: center;
}
```

> `p` is not centered, it's a block element and takes the full width. It's just the content/text that is centered.

## [margin](https://developer.mozilla.org/en-US/docs/Web/CSS/margin)

### Use Cases:

* Block element
* Width is set


### [Demo](https://codepen.io/n0rush/pen/MRXEKv)
```html
<div class='container'>
  <p> Hello to my demo </p>
</div>
```

```css
.container p {
  width: 200px;
  margin: 0 auto;
}
```

## [position](https://developer.mozilla.org/en-US/docs/Web/CSS/position)

### [Demo](https://codepen.io/n0rush/pen/VNdMRY)

```html
<main class="container">
  <div>I'm the content, I want to be horizontally centered. I'm the content, I want to be horizontally centered</div>
</main>
```

```css
.container {
  border: 1px solid red;
  position: relative;
  height: 300px
}

div {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
}
```

## [flex](https://developer.mozilla.org/en-US/docs/Web/CSS/flex)

### [Demo](https://codepen.io/n0rush/pen/xezXOJ)

```html
<main class="container">
  <div>This is a paragraph. I want to be horizontally centered</div>
  <div>This is a paragraph. I want to be horizontally centered</div>
  <div>This is a paragraph. I want to be horizontally centered</div>
</main>
```

```css
.container {
  display: flex;
  justify-content: center;
}

.container div {
  border: 1px solid red;
  margin-right: 10px;
}
```

# Vertical

## [line-height](https://developer.mozilla.org/en-US/docs/Web/CSS/line-height)

### Use Cases:

* Single line
* Inline or inline block

### [Demo](https://codepen.io/n0rush/pen/jRKGpG)

```html
<main class="container">
  <div>This is a paragraph. I want to be vertically centered</div>
</main>
```

```css
div {
  border: 1px solid red;
  height: 40px;
  line-height: 40px;
}
```

## [position](https://developer.mozilla.org/en-US/docs/Web/CSS/position)

### [Demo](https://codepen.io/n0rush/pen/KYeXxy)

```html
<main class="container">
  <div>I'm the content, I want to be vertically centered. I'm the content, I want to be vertically centered</div>
</main>
```

```css
.container {
  width: 150px;
  position: relative;
  height: 300px;
  border: 1px solid red;
}

div {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
}
```

## [flex](https://developer.mozilla.org/en-US/docs/Web/CSS/flex)

### [Demo](https://codepen.io/n0rush/pen/ROJLEW)

```html
<main class="container">
  <div>I'm the content, I want to be vertically centered. I'm the content, I want to be vertically centered</div>
</main>
```

```css
.container {
  width: 150px;
  display: flex;
  height: 300px;
  border: 1px solid red;
  align-items: center;
}
```

# Both

> Combination of the above horizontal and vertical centering solutions

# Conclusion

As long as you don't have browser compatibility consideration, try to take `display: flex` as your first choice. It's almost the silver bullet in centering.

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.