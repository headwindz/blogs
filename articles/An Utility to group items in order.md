# Toggle

## Toggle with only two choices

E.g. toggle a switch whose status can only be `on` and `off`. In the case, we can use `boolean`

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


## Toggle with more than two choices

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

The idea of grouping a list of items into subgroups is similar to the `toggle` example above where we make use of `quotient` and `remainder`

## Z Ordering

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

### Implementation

#### Background

* An array `arr`
* number of groups/colums `numOfColumns`

#### Pattern

```javascript
A B C D
E F G H
I
```

Given `numOfColumns`, the item with index `idx` should be placed on rowIndex `Math.floor(index / numOfColumns)` and columnIndex `idx % numOfColumns`. For the example, `G` has index `6` and `numOfColumns` is 4.  Therefore, 

* rowIndex of `G` would be `Math.floor(idx / numOfColumns) = Math.floor(6 / 4) = 1 `
* columnIndex of `G` would be `idx % numOfColumns = 6 % 4 = 2`

=> `G` would be on the third column of the second row. (Note: index starts from zero)


#### Code 

```javascript

const result = [];
arr.forEach((it, index) => {
  let rowIndex, columnIndex;
  rowIndex = Math.floor(index / numOfColumns);
  columnIndex = index % numOfColumns;
  result[rowIndex] = result[rowIndex] || [];
  result[rowIndex][columnIndex] = it;
});
```

## N Ordering

> top-to-bottom -> left-to-right

E.g. 

[1,2,3,4,5,6,7] with 3 groups will result in:

```
[[1, 4, 7], [2, 5], [3, 6]]
```

i.e.

```
1 4 7
2 5 
3 6
```

### Implementation

#### Background

* An array `arr`
* number of groups/colums `numOfColumns`

#### Thoughts

`Z` ordering is a bit counter-intuitive. However, it still follow a pattern similar to `N` ordering. The difference lays on

* The concept of row and column is the reverse of `N` ordering
* How to deal with `quotient` and `remainder` 

#### Pattern

```javascript
A C E G
B D F H
```

Given `numOfColumns`, the number of rows `numOfRows` that we need to have to hold the items would be `Math.ceil(arr.length / numOfColumns)`. The item with index `idx` should be placed on rowIndex `idx % numOfRows` and columnIndex `Math.floor(idx / numOfRows)`. For the example, `G` has index `6` and `numOfColumns` is 4.  Therefore, 

* `numOfRows` would be `Math.ceil(arr.length / numOfColumns) = Math.ceil(8 / 4) = 2` 
* rowIndex of `G` would be `idx % numOfRows = 6 % 2 = 0`
* columnIndex of `G` would be `Math.floor(idx / numOfRows) = Math.floor(6 / 2) = 3`

=> `G` would be on the fourth column of the first row. (Note: index starts from zero)

#### Code 

```javascript
let result = [];
const numOfRows = Math.ceil(arr.length / numOfColumns);
arr.forEach((it, index) => {
  let rowIndex, columnIndex;
  rowIndex = index % numOfRows;
  columnIndex = Math.floor(index / numOfRows);
  result[rowIndex] = result[rowIndex] || [];
  result[rowIndex][columnIndex] = it;
});
```

# Source Code

[Codepen](https://codepen.io/n0rush/pen/jRmbxX)
[github](https://github.com/n0ruSh/to-grid)
[npm](https://www.npmjs.com/package/to-grid)

# Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.
