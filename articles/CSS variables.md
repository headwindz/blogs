# CSS custom properties/variables

> Custom properties (sometimes referred to as CSS variables) are entities defined by CSS authors that contain specific values to be reused throughout a document.

Variables names are prefixed with `--`, like `--example-name`. Variables can be used in other declarations using the `var()` function, which allows you to define multiple fallback values when the given variable is not yet defined. [E.g.](https://codesandbox.io/s/css-variables-ypvdl)

```html
<p> hell paragraph </p>
<div> hello div </div>
```

```css
:root {
  --main-color: red;
}

p {
  color: var(--main-color);
  border: var(--not-present, 1px solid blue);
  padding: 10px;
}

div {
  margin-top: 10px;
  border: 1px solid var(--main-color);
}
```

# Why should you care?

## Variables are already available in CSS preprocessor like [Sass](https://sass-lang.com/) and [Less](http://lesscss.org/). 

E.g. in Sass

```sass
$main-color: red;

p {
  color: $main-color;
}

div {
  border: 1px solid $main-color;
}
```

will be converted to

```css
p {
  color: red;
}

div {
  border: 1px solid red;
}
```

However, CSS preprocessor variables suffer from a major drawback. The variables are static as they are compiled into CSS at compile time, making it impossible to change at run time.

## How CSS Variables helps

> CSS variables are subject to the cascade and inherit their value from their parent.

[E.g.](https://codepen.io/n0rush/pen/mZoqvq)

```html
<div class='box'> <p>hello</p> </div>
```

```css
:root {
  --gutter: 10px;
  --main-color: red;
}

* {
  box-sizing: border-box;
}

.box {
  --main-color: blue;
  padding: var(--gutter);
  width: 100px;
  height: 100px;
  text-align: center;
  border: 1px solid black;
  background-color: teal;
  background-clip: content-box;
  color: var(--main-color);
}

p {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0;
}

@media (min-width: 300px) {
  :root {
    --gutter: 20px;
  }
}}
```

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.