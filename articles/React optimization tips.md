In React, the component literally re-renders while its state or props change. While it makes sense as state represents internal data and props represent external data, it's not always the case. There are a few techniques which can prevent the component from unnecessary rendering. Let's go through the examples.

## Always re-render

```javascript
let count = 0;

class Button extends React.Component {
  render = () => {
    const { color } = this.props
    count++;
    return (
      <React.Fragment>
        <span> Render count: { count } </span>
        <button style={{ color }}> text </button>
      </React.Fragment>
    )
  }
}

class App extends React.Component {
  state = {
    color: 'red'
  }

  showRedButton = () => {
    this.setState({
      color: 'red'
    })
  }

  showGreenButton = () => {
    this.setState({
      color: 'green'
    })
  }

  render() {
    const { color } = this.state;
    const { showRedButton, showGreenButton } = this;
    return (
      <React.Fragment>
        <Button color={color}/>
        <button onClick={showRedButton}> red </button>
        <button onClick={showGreenButton}> green </button>
     </React.Fragment>)
  }
}

ReactDOM.render(<App/>, document.getElementById('root'))

```

[Online Code Sample](https://codepen.io/n0rush/pen/MZwwZr)

Each time we click the buttons (either `red` or `green`), the `count` increments, indicating the `Button` component is re-rendering. However, the only prop that the `Button` is dependent on is `color`. If the `color` keeps unchanging, the rendering is unnecessary. 

Goal: If we keep pressing the `red` button, we want the `count` to be unchanged as we dont want to re-render `Button` component since `color` remains the same.

## Technique 1: shouldComponentUpdate

```javascript

let count = 0;

class Button extends React.Component {
  shouldComponentUpdate(nextProps) {
    if(this.props.color === nextProps.color) {
      return false;
    }
    return true
  }
  
  render = () => {
    const { color } = this.props
    count++;
    return (
      <React.Fragment>
        <span> Render count: { count } </span>
        <button style={{ color }}> text </button>
      </React.Fragment>
    )
  }
}

class App extends React.Component {
  state = {
    color: 'red'
  }

  showRedButton = () => {
    this.setState({
      color: 'red'
    })
  }

  showGreenButton = () => {
    this.setState({
      color: 'green'
    })
  }

  render() {
    const { color } = this.state;
    const { showRedButton, showGreenButton } = this;
    return (
      <React.Fragment>
        <Button color={color}/>
        <button onClick={showRedButton}> red </button>
        <button onClick={showGreenButton}> green </button>
     </React.Fragment>)
  }
}

ReactDOM.render(<App/>, document.getElementById('root'))
```

[ShouldComponentUpdate Code Sample](https://codepen.io/n0rush/pen/KbpdGr)

## Technique 2: React.PureComponent

### How it works

```javascript
let count = 0;

class Button extends React.PureComponent {
  render = () => {
    const { color } = this.props
    count++;
    return (
      <React.Fragment>
        <span> Render count: { count } </span>
        <button style={{ color }}> text </button>
      </React.Fragment>
    )
  }
}

class App extends React.Component {
  state = {
    color: 'red'
  }

  showRedButton = () => {
    this.setState({
      color: 'red'
    })
  }

  showGreenButton = () => {
    this.setState({
      color: 'green'
    })
  }

  render() {
    const { color } = this.state;
    const { showRedButton, showGreenButton } = this;
    return (
      <React.Fragment>
        <Button color={color}/>
        <button onClick={showRedButton}> red </button>
        <button onClick={showGreenButton}> green </button>
     </React.Fragment>)
  }
}

ReactDOM.render(<App/>, document.getElementById('root'))
```

[PureComponent Code Sample](https://codepen.io/n0rush/pen/JwdYVG)

### Silver bullet?

`PureComponent` is basically `Component` with build-in `shouldComponentUpdate` hook. It seems that we should use `PureCompnent` at any cases since it has native rendering optimization?
Not really. `PureComponent` is using shadow equality for props and state comparison.

```javascript

class TodoList extends React.PureComponent {
  render() {
    const { todos } = this.props;
    return (<ul>
      {
        todos.map((it, index) => <li key={index}> {it} </li>)
      }
    </ul>)
  }
}

class App extends React.Component {
  inputRef = React.createRef()

  state = {
    todos: []
  }

  onAdd = () => {
    let { todos } = this.state;
    const text = this.inputRef.current.value;
    todos.push(text);
    this.setState({
      todos
    });
  }

  render() {
    const { todos } = this.state;
    const { onAdd, inputRef } = this
    return (
      <React.Fragment>
        <input ref={inputRef} type='text' placeholder='enter todo'/>
        <button onClick={onAdd}> add todo </button>
        <TodoList todos={todos}/>
      </React.Fragment>
    )
  }
}

ReactDOM.render(<App/>, document.getElementById('root'))

```

[Anti PureComponent](https://codepen.io/n0rush/pen/wRKKZp)

In the example, `TodoList` will never re-render as `todos` (which is the state from `App` and is passed in as props to `TodoList`) always has the same reference.

### How to make it work?

```javascript

class TodoList extends React.PureComponent {
  render() {
    const { todos } = this.props;
    return (<ul>
      {
        todos.map((it, index) => <li key={index}> {it} </li>)
      }
    </ul>)
  }
}

class App extends React.Component {
  inputRef = React.createRef()

  state = {
    todos: []
  }

  onAdd = () => {
    let { todos } = this.state;
    const text = this.inputRef.current.value;
    this.setState({
      todos: [...todos, text]
    });
  }

  render() {
    const { todos } = this.state;
    const { onAdd, inputRef } = this
    return (
      <React.Fragment>
        <input ref={inputRef} type='text' placeholder='enter todo'/>
        <button onClick={onAdd}> add todo </button>
        <TodoList todos={todos}/>
      </React.Fragment>
    )
  }
}

ReactDOM.render(<App/>, document.getElementById('root'))

```

[PureComponent reference](https://codepen.io/n0rush/pen/wRKMMj)

It now works as we now make a new `todos` array each time we call the `setState`

## React.memo - for functional component

### How it works

```javascript
let count = 0;

const Button = React.memo(function(props) {
  const { color } = props
  count++;
  return (
    <React.Fragment>
       <span> Render count: { count } </span>
       <button style={{ color }}> text </button>
      </React.Fragment>
    )
})

class App extends React.Component {
  state = {
    color: 'red'
  }

  showRedButton = () => {
    this.setState({
      color: 'red'
    })
  }

  showGreenButton = () => {
    this.setState({
      color: 'green'
    })
  }

  render() {
    const { color } = this.state;
    const { showRedButton, showGreenButton } = this;
    return (
      <React.Fragment>
        <Button color={color}/>
        <button onClick={showRedButton}> red </button>
        <button onClick={showGreenButton}> green </button>
     </React.Fragment>)
  }
}

ReactDOM.render(<App/>, document.getElementById('root'))
```

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.
