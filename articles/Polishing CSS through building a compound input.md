In the post I will list the steps taken to build a compound input component and some interesting CSS observations made through the process.

## Round one

```html
<input class='input protocol' placeholder='enter protocol' type='text'/>
<input class='input host' placeholder='enter host' type='text'/>
```

```css
.input {
  padding: 5px;
  border: 1px solid #d1c9c9;
}
.input:focus {
  border-color: #3c5ed9;
}
```

![outline](https://user-images.githubusercontent.com/7504237/59962072-0d34eb80-9513-11e9-8e9a-ab701884ee67.gif)

There are two problems with the above solution.

* A line is drawn around elements to make the input "stand out" when the input is clicked. 


<details><summary><b>Why and How</b></summary>
<p>
The line is [outline](https://developer.mozilla.org/en-US/docs/Web/CSS/outline). It can be removed by setting `outline: none`.
</p>
</details>

* Currently there are some spaces between the two inputs while I expect them to sit near each other.

<details><summary><b>Why and How</b></summary>
<p>
No margins are applied to the input elements. The space is the character from html code. There are a few solutions we can apply to solve the problem.

* Remove the character

E.g.

```html
<input class='input protocol' placeholder='enter protocol' type='text'/><input class='input host' placeholder='enter host' type='text'/>
```

* Apply negative margin to one of the input elements to shift its position

* Wrap the inputs with a parent container and set font size to 0 on parent.
</p>
</details>


We then result in the following solution:

```html
<div class='compound-input'>
  <input class='input protocol' placeholder='enter protocol' type='text'/>
  <input class='input host' placeholder='enter host' type='text'/>
</div>
```

```css
.compound-input {
  font-size: 0;
}

.input {
  outline: none;
  padding: 5px;
  border: 1px solid #d1c9c9;
}

.input:focus {
  border-color: #3c5ed9;
}
```

## Round two

![borders](https://user-images.githubusercontent.com/7504237/59962079-28076000-9513-11e9-9d4a-0e2f4f9d7b4a.png)

Now that the two inputs are near each other. However, the right border of the left input and the left border of the right input adds up and it looks inharmonic. Setting a negative `margin-right` to the first input to shift it towards the second input should fix the problem, as now the two borders stack.

```html
<div class='compound-input'>
  <input class='input protocol' placeholder='enter protocol' type='text'/>
  <input class='input host' placeholder='enter host' type='text'/>
</div>
```

```css
.compound-input {
  font-size: 0;
}

.input {
  outline: none;
  padding: 5px;
  border: 1px solid #d1c9c9;
}

.input:focus {
  border-color: #3c5ed9;
}

.input:first-child {
  margin-right: -1px;
}
```

## Round three

![border stack](https://user-images.githubusercontent.com/7504237/59962086-381f3f80-9513-11e9-8d55-c140829be087.gif)

By setting negative `margin-left: -1px` to the first input element, the right-border of first input element is now always missing. Clicking the first input will no longer has a right border highlight effect. The first idea that comes up in mind is to set the [z-index](https://developer.mozilla.org/en-US/docs/Web/CSS/z-index) to the element that has [focus state](https://developer.mozilla.org/en-US/docs/Web/CSS/:focus). To make it work, we also have to set the `position: relative` to both inputs as `z-index` only works for a positioned box (that is, one with any position other than static) 

[Final Solution](https://codepen.io/n0rush/pen/BgWddO)

```html
<div class='compound-input'>
  <input class='input protocol' placeholder='enter protocol' type='text'/>
  <input class='input host' placeholder='enter host' type='text'/>
</div>
```

```css
.compound-input {
  font-size: 0;
}

.input {
  outline: none;
  padding: 5px;
  border: 1px solid #d1c9c9;
  position: relative;
}

.input:focus {
  border-color: #3c5ed9;
}

.protocol {
  margin-right: -1px;
}

.input:first-child:focus {
  z-index: 10;
}
```

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe