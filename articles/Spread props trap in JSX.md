# Spread props trap in JSX

```jsx
// A normal Text Component
import React from 'react';

export default function Text(props) {
	const { text, ...restProps } = props;
	// You have no idea what to expect in restProps as it's passed down from parent
	return <div style={{ color: 'red'}} {...restProps}> { text } </div>  
}
```

```jsx
import React, { Fragment } from 'react';
import ReactDOM from 'react-dom';

export default function withHighlight(Comp) {
	return (props) => {
		return <Comp style={{ fontSize: 20 }} {...props} />
	}
}

// what's the color ? style overwritten
const Comp = withHighlight(Text); 
ReactDom.render(
  <Fragment>
	  <Text text='abc'/>
	  <Comp text='abc'/>
  </Fragment>,
  document.getElementById('root')
)
```

> Solution: merge props if necessary  

```jsx
// A normal Text Component
import React from 'react';

export default function Text(props) {
	const { text, style, ...restProps } = props;
	return <div style={{ color: 'red', ...(style == null ? {} : style)}} {...restProps}>
    { text }
  </div>  
}
```

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.
