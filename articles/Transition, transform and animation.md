# [Transition](https://developer.mozilla.org/en-US/docs/Web/CSS/transition)

> Transitions enable you to define the transition between two states of an element when the state changes.

The `transition` CSS property is a shorthand property for

* [transition-property](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-property)
* [transition-duration](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-duration)
* [transition-timing-function](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-timing-function)
* [transition-delay](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-delay)

## Syntax

```css
transition: <single-transition> = [ none | <single-transition-property> ] || <time> || <timing-function> || <time>
```

E.g. 

```css
transition: border-radius 0.3s linear;
```

If you need to apply two different transitions to two different properties, you can do that by adding multiple transition rules, each separated by a comma. E.g.

```css
transition: border-radius 0.3s linear,  background-color 0.4s ease-in;
```

## Example

```html
<button> Hover me </button>
```

```css

button {
  background-color: orange;
  border: 0;
  color: white;
  font-size: 1em;
  padding: 0.3em 1em;
  transition: border-radius 0.3s linear,  background-color 0.4s ease-in;
}

button:hover {
  background-color: teal;
  border-radius: 10px;
}
```

<iframe height="265" style="width: 100%;" scrolling="no" title="Button with transition" src="//codepen.io/n0rush/embed/zVmoar/?height=265&theme-id=dark&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/n0rush/pen/zVmoar/'>Button with transition</a> by 不吃猫的鱼
  (<a href='https://codepen.io/n0rush'>@n0rush</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>  <span>See the Pen <a href="https://codepen.io/n0rush/pen/zVmoar/">
  Button with transition</a> by 不吃猫的鱼 (<a href="https://codepen.io/n0rush">@n0rush</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

# [Transform](https://developer.mozilla.org/en-US/docs/Web/CSS/transform)

> The `transform` CSS property lets you rotate, scale, skew, or translate an element. It modifies the coordinate space of the CSS visual formatting model.

## [Rotate](https://developer.mozilla.org/en-US/docs/Web/CSS/rotate)

> The `rotate` CSS property allows you to specify rotation transforms individually and independently of the transform property

```html
<div class='item thirty'> Rotate 30deg </div>
<div class='item minus-thirty'> Rotate -30deg </div>
```

```css
* {
  box-sizing: border-box;  
}

.item {
  background-color: teal;
  width: 100px;
  height: 100px;
  display: flex;
  align-items: center;
  padding: 10px;
  margin: 50px;
}

.thirty {
  transform: rotate(30deg);
}

.minus-thirty {
  transform: rotate(-30deg);
}
```

<iframe height="265" style="width: 100%;" scrolling="no" title="rotate" src="//codepen.io/n0rush/embed/RzeodM/?height=265&theme-id=dark&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/n0rush/pen/RzeodM/'>rotate</a> by 不吃猫的鱼
  (<a href='https://codepen.io/n0rush'>@n0rush</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

### Making a badge

```html
<div class='card'>
  <div class='badge'> Prime </div>
</div>
```

```css
* {
  box-sizing: border-box;  
}

.card {
  background-color: teal;
  width: 200px;
  height: 100px;
  margin: 50px;
  position: relative;
  overflow: hidden;
}

.badge {
  position: absolute;
  background-color: #dfc9c9;
  width: 80px;
  height: 15px;
  font-size: 13px;
  color: blue;
  left: 0;
  top: 0;
  text-align: center;
  transform: rotate(-45deg) translateX(-23px)
}
```

<iframe height="265" style="width: 100%;" scrolling="no" title="rotate" src="//codepen.io/n0rush/embed/zVmNKw/?height=265&theme-id=dark&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/n0rush/pen/zVmNKw/'>rotate</a> by 不吃猫的鱼
  (<a href='https://codepen.io/n0rush'>@n0rush</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

## [Transform](https://developer.mozilla.org/en-US/docs/Web/CSS/transform)

> The transform CSS property lets you rotate, scale, skew, or translate an element. It modifies the coordinate space of the CSS visual formatting model.

```html
<div class='wrapper'>
  <div class='item x-30'> translateX(30px)</div>
</div>
<div class='wrapper'>
  <div class='item y-30'>translateY(30px)</div>
</div>
<div class='wrapper'>
  <div class='item z-50'>translateZ(50px) -> closer to user -> getting larger</div>
</div>
```

```css
* {
  box-sizing: border-box;  
}

.wrapper {
  border: 1px dashed black;
  margin-bottom: 100px;
}

.item {
  background-color: teal;
  width: 100px;
  height: 100px;
}

.x-30 {
  transform: translateX(30px)
}

.y-30 {
  transform: translateY(30px)
}

.z-50 {
  font-size: 14px;
  transform: perspective(500px) translateZ(50px)
}
```

<iframe height="265" style="width: 100%;" scrolling="no" title="rotate" src="//codepen.io/n0rush/embed/pXxRaE/?height=265&theme-id=dark&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/n0rush/pen/pXxRaE/'>rotate</a> by 不吃猫的鱼
  (<a href='https://codepen.io/n0rush'>@n0rush</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>