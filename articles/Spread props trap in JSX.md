# Spread props trap in JSX

```jsx
// A normal Text Component
import React from 'react';

export default function Text(props) {
	const { text, ...restProps } = props;
	// what do you have in restProps? You have no idea inside component implementation. It is from parent invoker
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
ReactDom.render(<Fragment>
	<Text text='abc'/>
	<Comp text='abc'/>
</Fragment>, document.getElementById('root'))
```

> Sol: merge props if necessary  

```jsx
// A normal Text Component
import React from 'react';

export default function Text(props) {
	const { text, style, ...restProps } = props;
	return <div style={{ color: 'red', ...(style == null ? {} : style)}} {...restProps}> { text } </div>  
}
```

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.
