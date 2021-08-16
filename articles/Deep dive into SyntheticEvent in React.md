# Deep dive into SyntheticEvent in React

> `SyntheticEvent` is a wrapper that forms part of React’s Event System

## Why it's useful

- Cross-browser: It wraps the brower's native event through `nativeEvent` attribute and provices a uniform api and consistent behavior on top level
- Better performance: Events are [delegated](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events) to document through bubbling. [Note Event Pooling is removed in React 17](https://reactjs.org/blog/2020/08/10/react-v17-rc.html#no-event-pooling)

## What it's like

Every `SyntheticEvent` object has the following attributes

```bash
boolean bubbles
boolean cancelable
DOMEventTarget currentTarget // the element that the handler is registered
boolean defaultPrevented
number eventPhase
boolean isTrusted
DOMEvent nativeEvent
void preventDefault()
boolean isDefaultPrevented()
void stopPropagation()
boolean isPropagationStopped()
void persist()
DOMEventTarget target // the element that the handler is triggered
number timeStamp
string type
```

## When native and synthetic events are mixed

Synthetic events are delegated to `document` node. Therefore native events are triggered first and the events bubble up to doucment, afther which the synthetic events are triggerd.

```javascript
import React, { useEffect } from "react";

export default function App() {
  useEffect(() => {
    document.addEventListener('click', e => {
      // @4 stop parent native propogation
      // e.stopPropagation();
      console.log('[document] native dom event triggered');
    });
    const parentDiv = document.getElementById('parent');
    parentDiv.addEventListener('click', e => {
      // @1 stop parent native propogation
      // e.stopPropagation();
      console.log('[parent div] native dom event triggered');
    });
  }, []);

  const onParentClick = e => {
    // @2 stop parent propogation
    // e.stopPropagation();
    console.log('[parent div] synthetic event triggered');
  };

  const onChildClick = e => {
    // @3 stop child propogation
    // e.stopPropagation();
    console.log('[child button] synthetic event triggered');
  };

  return <div id="parent" onClick={onParentClick}>
    <button onClick={onChildClick}>child</button>
  </div>;
}
```

The output of clicking `child` button with various setting will be:

#### No stopPropogation - Output: 

```bash
[parent div] native dom event triggered
[child button] synthetic event triggered
[parent div] synthetic event triggered
[document] native dom event triggered
```

native dom event is registered later as it's called after component is mounted.


#### stopPropogation on parent's native node (i.e. @1) - Output: 

```bash
[parent div] native dom event triggered
```

No bubbling up to document as if there is no click event happening on document => synthetic events are NOT triggered

#### stopPropogation on parent's synthetic event (i.e. @2) - Output: 

```bash
[parent div] native dom event triggered
[child button] synthetic event triggered
[parent div] synthetic event triggered
[document] native dom event triggered
```

#### stopPropogation on child's synthetic event (i.e. @3) - Output: 

```bash
[parent div] native dom event triggered
[child button] synthetic event triggered
[document] native dom event triggered
```

#### stopPropogation on document's native event (i.e. @4) - Output: 

```javascript
[parent div] native dom event triggered
[child button] synthetic event triggered
[parent div] synthetic event triggered
[document] native dom event triggered
```

## Key points

- Listen on `document` if you want to receive events after all React handlers. Listen anywhere else in order to receive before React handlers
- React event handlers will always execute after native capture handlers


[Code](https://codesandbox.io/s/synthetic-event-ruv3f?file=/src/App.js)

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.
