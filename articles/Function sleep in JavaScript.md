> In Java, [Thread.sleep](https://docs.oracle.com/javase/tutorial/essential/concurrency/sleep.html) causes the current thread to suspend execution for a specified period. 

This is an efficient means of making processor time available to the other threads of an application or other applications that might be running on a computer system. However, JavaScript does NOT have such a native implementation. Thanks to [async function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function), we can emulate the behavior: 

```javascript

async function sleep(interval) {
  return new Promise(resolve => {
    setTimeout(resolve, interval);
  })
}
```

## Use case: print number 1 to 5 per second in consecutive way.

### [Async & Await implementation](https://codepen.io/n0rush/pen/XLropN)

```javascript
async function one2FiveInAsync() {
  for(let i = 1; i <= 5; i++) {
    console.log(i);
    await sleep(1000)
  }
}

one2FiveInAsync();
```

### [Promise implementation](https://codepen.io/n0rush/pen/orvJWp)

```javascript

function one2FiveInPromise() {
  function logAndSleep(i) {
    console.log(i);
    if (i === 5) {
      return;
    }
    return sleep(1000).then(() => logAndSleep(i + 1));
  }

  logAndSleep(1);
}

one2FiveInPromise();
```

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.