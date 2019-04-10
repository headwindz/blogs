# Toggle

## Toggle with only two choices

E.g. toggle a switch whose status can only on `on` and `off`. In the case, we can use `boolean`

```javascript
let status = true;
function toggle() {
  status = !status;
}

for(let i = 0; i < 10; i++) {
  toggle();
  console.log(status ? 'on' : 'off');
}
```


## Toggle with only more than two choices

E.g. toggle traffic lights. In the case, we can use a counter and increment the counter at each toggle and use `remainder` to get the active light.

```javascript

let counter = 0;
function toggle() {
  counter++
}

function getActiveLight() {
  let remainder = counter % 3;
  switch (remainder) {
    case 0:
      return 'red';
    case 1:
      return 'yellow';
    case 2:
      return 'green';
  }
}

for(let i = 0; i < 10; i++) {
  toggle();
  console.log(getActiveLight());
}
```


# Grouping

The idea of grouping a list of items into subGroups is similar to the `toggle` example above where we make use of `quotient` and `remainder`

## One use case

There is a list of strings and the list needs to be display on a panel with different ordering. 

# Z Ordering

> left-to-right, top-to-bottom

E.g. 

[1,2,3,4,5,6,7] with 3 groups will result in:

```
[[1, 2, 3], [4, 5, 6], [7]]
```

i.e.

```
1 2 3
4 5 6
7
```

# N Ordering

> top-to-bottom -> left-to-right

E.g. 

[1,2,3,4,5,6,7] with 3 groups will result in:

```
[1, 4, 7], [2, 5], [3, 6]])
```

i.e.

```
1 4 7
2 5 
3 6
```

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.
