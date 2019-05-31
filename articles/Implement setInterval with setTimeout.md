## setInterval vs setTimeout

* [setTimeout](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout): sets a timer which executes a function or specified piece of code once the timer expires.
* [setInterval](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setInterval): *repeatedly* calls a function or executes a code snippet, with a fixed time delay between each call

*setInterval* can be implemented as `recurisve` *setTimeout* calls. 

# [setInterval implementation with setTimeout](https://codepen.io/n0rush/pen/RmqObZ)

with signature 

```typescript
function _setInterval(fn: Function, delay: number): number;
```

The call stack will be:

--delay--> fn() --delay--> fn() --delay--> fn() ...

To delay the execution of `fn`, we could simply use *setTimeout*. Therefore, we create a `wrapper` function which encapsulates the logic which executes the original function.

```javascript
function _setInterval(fn, delay) {
  // wrap the original function, recursively call the wrapper function with setTimeout 
  const wrapper = () => {
    fn();
    return setTimeout(wrapper, delay)
  }

  setTimeout(wrapper, delay);
}

_setInterval(console.log.bind(null, 'hello world'), 1000);
```

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.