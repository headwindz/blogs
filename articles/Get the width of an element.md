> Each html element exposes some properties to facilitate fetching element dimensions 

## [clientWidth](https://developer.mozilla.org/en-US/docs/Web/API/Element/clientWidth)

> `clientWidth` is the inner width (ie. the space inside an element including padding but excluding borders and scrollbar)

* `clientWidth` does NOT only works with `inline` elements. It is zero for `inline` elements.
* This property will round the value to an integer.

## [offsetWidth](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/offsetWidth)

> `offsetWidth` is the outer width (ie. the space occupied by the element, including padding and borders)

## [getBoundingClientRect](https://developer.mozilla.org/en-US/docs/Web/API/Element/getBoundingClientRect) 

* This property will round the value to an integer.

> `getBoundingClientRect` method returns the size of an element and its position relative to the viewport. (link to position fixed with absolute)

* `getBoundingClientRect` only returns the dimensions, but also the position which can be pretty useful. E.g. [Implement position:fixed with position:absolute](https://github.com/n0ruSh/the-art-of-reading/blob/master/articles/Fixed%20with%20absolute.md).
* This method will return a fractional value.


```html
<div id='app'/>
```

```css
.box {
  width: 200.5px;
  height: 100px;
  border: 5px solid red;
  padding: 5px;
  background-image: linear-gradient(to right, teal, orange)
}
```

```javascript
const { Fragment } = React;

function BlockBox(props) {
  const divRef = React.createRef();
  const [clientWidth, setClientWidth] = React.useState(0);
  const [offsetWidth, setOffsetWidth] = React.useState(0);
  const [boundingWidth, setBoundingWidth] = React.useState(0);
  React.useEffect(() => {
    const { clientWidth: cw, offsetWidth: ow } = divRef.current;
    setClientWidth(cw);
    setOffsetWidth(ow);
    setBoundingWidth(divRef.current.getBoundingClientRect().width);
  }, []);
  
  
  function renderInfo() {
    return <>
      <div>
        bounding width: {boundingWidth}
      </div>
      <div>
        client width: {clientWidth}
      </div>
      <div>
        offset width: {offsetWidth}
      </div>
    </>
  }
  
  return <div ref={divRef} className='box'>
    { renderInfo() }
  </div>;
}

function InlineBox(props) {
  const spanRef = React.createRef();
  const [clientWidth, setClientWidth] = React.useState(0);
  const [offsetWidth, setOffsetWidth] = React.useState(0);
  const [boundingWidth, setBoundingWidth] = React.useState(0);
  React.useEffect(() => {
    const { clientWidth: cw, offsetWidth: ow } = spanRef.current;
    setClientWidth(cw);
    setOffsetWidth(ow);
    setBoundingWidth(spanRef.current.getBoundingClientRect().width);
  }, []);
  
  
  function renderInfo() {
    return <>
      <span>
        bounding width: {boundingWidth}
      </span>
      <span>
        client width: {clientWidth}
      </span>
      <span>
        offset width: {offsetWidth}
      </span>
    </>
  }
  
  return <span ref={spanRef} className='box'>
    { renderInfo() }
  </span>;
}

ReactDOM.render(<Fragment><BlockBox/><br/><InlineBox/></Fragment>, document.getElementById('app'))
```

[Comparison](https://codepen.io/n0rush/pen/dBEwzO)

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.