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

<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="result" data-user="n0rush" data-slug-hash="zVmoar" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Button with transition">
  <span>See the Pen <a href="https://codepen.io/n0rush/pen/zVmoar/">
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

<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="result" data-user="n0rush" data-slug-hash="RzeodM" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="rotate">
  <span>See the Pen <a href="https://codepen.io/n0rush/pen/RzeodM/">
  rotate</a> by 不吃猫的鱼 (<a href="https://codepen.io/n0rush">@n0rush</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

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

<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="result" data-user="n0rush" data-slug-hash="zVmNKw" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="badge with rotate">
  <span>See the Pen <a href="https://codepen.io/n0rush/pen/zVmNKw/">
  badge with rotate</a> by 不吃猫的鱼 (<a href="https://codepen.io/n0rush">@n0rush</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## [Translate](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function/translate)

> The translate() CSS function repositions an element in the horizontal and/or vertical directions. Its result is a <transform-function> data type.

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

<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="result" data-user="n0rush" data-slug-hash="pXxRaE" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="transform">
  <span>See the Pen <a href="https://codepen.io/n0rush/pen/pXxRaE/">
  transform</a> by 不吃猫的鱼 (<a href="https://codepen.io/n0rush">@n0rush</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## [Scale](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function/scale)

>The scale() CSS function defines a transformation that resizes an element on the 2D plane. Because the amount of scaling is defined by a vector, it can resize the horizontal and vertical dimensions at different scales

> A transform is made around a point of origin. This point servers as the axis of rotation, or the spot where scaling or skewing begins.

```html
<div class='wrapper'>
  <div class='item one'> scale(0.5)</div>
</div>

<div class='wrapper'>
  <div class='item two'> scale(0.5, 0.7)</div>
</div>

<div class='wrapper'>
  <div class='item three'> origin(0,0) scale(0.5)</div>
</div>
```

```css
* {
  box-sizing: border-box;  
}

.wrapper {
  border: 1px dashed black;
  width: 200px;
  height: 200px;
  position: relative;
  margin-bottom: 20px;
}

.item {
  background-color: rgba(209, 194, 194, 0.3);
  background-clip: padding-box;
  width: 200px;
  height: 200px;
  position: absolute;
  left: -1px;
  top: -1px;
  line-height: 200px;
  text-align: center;
}

.one {
  transform: scale(0.5);
}

.three {
  transform: scale(0.5);
  transform-origin: 0 0
}
```

<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="result" data-user="n0rush" data-slug-hash="GbwowZ" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="scale">
  <span>See the Pen <a href="https://codepen.io/n0rush/pen/GbwowZ/">
  scale</a> by 不吃猫的鱼 (<a href="https://codepen.io/n0rush">@n0rush</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## [Skew](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function/skew)

> The skew() CSS function defines a transformation that skews an element on the 2D plane


```html
<div class='wrapper'>
  <div class='item one'> skew(15deg)</div>
</div>
```

```css
* {
  box-sizing: border-box;  
}

.wrapper {
  border: 1px dashed black;
  width: 200px;
  height: 200px;
  position: relative;
  margin-bottom: 20px;
}

.item {
  background-color: rgba(209, 194, 194, 0.3);
  background-clip: padding-box;
  width: 200px;
  height: 200px;
  position: absolute;
  left: -1px;
  top: -1px;
  line-height: 200px;
  text-align: center;
}

.one {
  transform: skew(15deg)
}
```

<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="result" data-user="n0rush" data-slug-hash="VJVaja" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="skew">
  <span>See the Pen <a href="https://codepen.io/n0rush/pen/VJVaja/">
  skew</a> by 不吃猫的鱼 (<a href="https://codepen.io/n0rush">@n0rush</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

# [Animation](https://developer.mozilla.org/en-US/docs/Web/CSS/animation)

> The animation shorthand CSS property applies an animation between styles.

It is a shorthand for

* [animation-name](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-name)
* [animation-duration](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-duration)
* [animation-timing-function](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-timing-function)
* [animation-delay](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-delay)
* [animation-iteration-count](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-iteration-count)

Animations in CSS contain two parts: the `@keyframes` at rule, which defines an animation, and the `animation` property, which applies that animation to an element.

```html
<div class="box"/> 
```

```css
@keyframes over-and-back {
  0% {
    background-color: hsl(0, 50%, 50%);
    transform: translate(0)
  }

  50% {
    transform: translate(100px)
  }

  100% {
    background-color: hsl(270, 50%, 90%);
    transform: translate(0)
  }
}

.box {
  width: 100px;
  height: 100px;
  background-color: red;
  animation: over-and-back 3s ease-in 3;
}
```

<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="css,result" data-user="n0rush" data-slug-hash="agQZzm" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="animation">
  <span>See the Pen <a href="https://codepen.io/n0rush/pen/agQZzm/">
  animation</a> by 不吃猫的鱼 (<a href="https://codepen.io/n0rush">@n0rush</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## Flying card

```html
<div class='flying-card'>
  <h4> Cluck Norries </h4>
  <p> Every brood has its brawler. Cluck Norris is our feistiest hen, frequently picking fights with other hens about laying territory and foraging space </p> 
```

```css
.flying-card {
  border: 1px solid black;
  max-width: 300px;
  padding: 10px;
  box-sizing: border-box;
  box-shadow: 0.2em 0.5em 1em rgba(0,0,0,0.3);
  animation: fly-in 2s ease-in;
  perspective: 500px;
}

p {
  color: hsl(210, 15%, 20%)
}

@keyframes fly-in {
  0% {
    transform: translateZ(-800px) rotateY(90deg);
    opacity: 0;
  }
  56% {
    transform: translateZ(-160px) rotateY(80deg);
    opacity: 1;
  }
  100% {
    transform: translateZ(0) rotateY(0);
  }
}
```

<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="css,result" data-user="n0rush" data-slug-hash="rEQeQN" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="flying card">
  <span>See the Pen <a href="https://codepen.io/n0rush/pen/rEQeQN/">
  flying card</a> by 不吃猫的鱼 (<a href="https://codepen.io/n0rush">@n0rush</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.