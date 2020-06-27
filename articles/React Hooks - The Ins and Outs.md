# React Hooks - The Ins and Outs

> [Hooks](https://reactjs.org/docs/hooks-intro.html) are functions that let you “hook into” React state and lifecycle features from function components.

## Why hooks

### How did we reuse stateful logic - [HOC](https://reactjs.org/docs/higher-order-components.html)

E.g.

```javascript
import { withRouter } from "react-router-dom";
import { Form } from 'antd';

function A(props) {
  const {
    match: { params },
    history,
    form
  } = props;

  return <div> test </div>;
}

export default withRouter(Form.create()(A))
```

There are a few drawbacks with HOC.

- **wrapper hell** of components. HOC actually returns a new component that wraps the original component.
- unclear dependencies
  - from the view of Componet **A**, we have no way to clearly know **this.props.form **is from **Form.create** HOC, and **this.props.history** is from **withRouter** HOC
- naming conflicts
  - imagine that **Form.create** and **withRouter** both inject a prop with the same name, which can easily happen as they are two seperate HOCs that are properly maintained by two teams

### How can we improve with Hooks

```javascript
import { useHistory } from "react-router-dom";
import { Form } from 'antd';

export default function A(props) {
  const [form] = Form.useForm();
  const history = useHistory();

  return <div> test </div>;
}
```

- no wrapper any more as they are all at the same level
- clear dependencies as they are from separate hooks call: **form** from **useForm** and **history** from **useHistory**
- no naming conflicts as they are from seperate hooks and you can rename hooks return values

## Hooks in practice

To implement a controlled **Input** element with **reset** functionality, we can do it [this way](https://codesandbox.io/s/controlled-input-with-reset-2js6i?file=/src/App.js):

```javascript
import React, { useState } from "react";
import { Input, Button } from 'antd';

export default function App() {
  const initialValue = 'please enter something';
  const [inputValue, setInputValue] = useState(initialValue);

  return (
    <div className="App">
    	// note the value is from e.target
      <Input value={inputValue} onChange={e => setInputValue(e.target.value)}/>
      <Button onClick={() => setInputValue(initialValue)}> Reset </Button>
    </div>
  );
}
```

There are a couple of stateful logic that we can abstract and resue.

- value fetch -  e.target.value
- reset logic - reset to initial value

### custom hook - [useEventTarget](https://codesandbox.io/s/useeventtarget-z18i3?file=/src/useEventTarget.js)

```javascript
import { useState, useCallback } from "react";

export default function useEventTarget(initialValue = "") {
  const [value, setValue] = useState(initialValue);
  const onChange = useCallback(e => setValue(e.target.value), []); // reusable value fetch logic
  const reset = useCallback(() => setValue(initialValue), [initialValue]); // resuable reset logic

  return {
    value,
    onChange,
    reset
  };
}
```

The above example can then be [refactored](https://codesandbox.io/s/useeventtarget-z18i3?file=/src/App.js) with custom useEventTarget hook:

```javascript
import React from "react";
import { Input, Button } from "antd";
import useEventTarget from "./useEventTarget";

export default function App() {
  const { value, onChange, reset } = useEventTarget("please enter something");

  return (
    <div className="App">
      <Input value={value} onChange={onChange} />
      <Button onClick={reset}> Reset </Button>
    </div>
  );
}
```

## Mental overhead

> Hooks is NOT a substitue for class lifecycles though useEffect can simulate what lifecycles do. Hooks facilitates reusing stateful logic but it comes with costs.

Assume that I need a page that display **count** which is from zero and increment by one per second.

```javascript
import React, { useState, useEffect } from 'react';

export default function SetInterval() {
  const [ count, setCount ] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      console.log("interval count", count); // count is always 0
      setCount(count + 1);
    }, 1000);
    return () => {
      clearInterval(interval)
    }
  }, []);

  return <div> normal count is { count } </div>;
}
```

With [the code above](https://codesandbox.io/s/setinterval-wt720?file=/src/App.js), the count on the page will stay on 1.  This is due to the fact that **useEffect** call only runs on mount (as **useEffect** dependency is set to [] on line 14) and the **count** value at that point is 0 (i.e the initial value we set with **useState** call on line 4). Each time the interval function is called, the **count** value will always be 0 as it's a closure variable. Therefore, setCount(count + 1) will be setCount(1).

### [Solution 1 - useEffect dependencies](https://codesandbox.io/s/setinterval-with-dependency-36fb5?file=/src/App.js:0-355)

> Specify the dependencies if your useEffect depends on outer variables

```javascript
import React, { useState, useEffect } from "react";

export default function SetInterval() {
  const [count, setCount] = useState(0);
  useEffect(() => {
    const interval = setInterval(() => {
      console.log('interval count', count); // increment by one per run
      setCount(count + 1);
    }, 1000);

    return () => {
      // each time count value changes (i.e per second, clear the interval and init one with new count value
      console.log('interval clear', count); 
      clearInterval(interval);
    }
  }, [count]);

  return <div> dep count is {count} </div>;
}
```

### [Solution 2 - use state callback](https://codesandbox.io/s/setinterval-with-prev-vsi1y)

```javascript
import React, { useState, useEffect } from "react";

export default function SetInterval() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      // it doesnt matter what count was, it tells React to increment by one with previous value
      setCount(a => a + 1);
    }, 1000);

    return () => {
      clearInterval(interval);
    };
  }, []);

  return <div> prev normal count is {count} </div>;
}
```

### [Solution 3 - useRef](https://codesandbox.io/s/setinterval-with-ref-qldj0?file=/src/App.js)

```javascript
import React, { useRef, useState, useEffect } from "react";

export default function SetInterval() {
  const [count, setCount] = useState(0);
  const ref = useRef();
  ref.current = count;

  useEffect(() => {
    const interval = setInterval(() => {
      console.log("interval count", ref.current);
      setCount(ref.current + 1);
    }, 1000);

    return () => {
      clearInterval(interval);
    };
  }, []);

  return <div> ref count is {count} </div>;
}
```

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.