# Sticky footer

> A sticky footer pattern is one where the footer of your page 'sticks' to the bottom of the browser window.

- Footer sticks to the bottom of the viewport when content is short
- If the content of the page extends past the viewport bottom, the footer then sits below the content as normal

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <link rel="stylesheet" href="./index.css" />
  </head>
  <body>
    <div class="container">
      <div class="header">Header</div>
      <div class="main">Main</div>
      <div class="footer">Footer</div>
    </div>
  </body>
</html>
```

```css
.container {
  display: flex;
  flex-direction: column;
  height: 100vh;
}

.header {
  height: 100px;
  background-color: red;
  flex-shrink: 0;
}

.main {
  flex: 1 0 auto;
  background-color: green;
  /* height: 1100px; */
}

.footer {
  height: 100px;
  background-color: teal;
  flex-shrink: 0;
}
```

[Code Example](https://codesandbox.io/s/sticky-footer-d56em)

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe