## The Uncontrolled

> An `Uncontrolled` Component is one that stores and maintains its own state internally. 

* A [`ref`](https://reactjs.org/docs/refs-and-the-dom.html) is used to find its current value when you need it.
* [React](https://reactjs.org/docs/uncontrolled-components.html) doesn't recommend this pattern but it's useful when developers only care about the final state rather than the 
intermediate state of the comopnent. The following is an [example](https://codepen.io/n0rush/pen/RORrRa) of a `Switch` implemented in `uncontrolled` way.

```javascript
class Switch extends React.Component {
  constructor(props) {
    super(props);
    // maintain its own state
    this.state = {
      checked: false
    };
  }
  
  // expose state data for parent component to access
  get value() {
    return this.state.checked;
  }
  
  toggle = () => {
    this.setState((prevState) => {
       return {
         checked: !prevState.checked
       }
    })
  }

  render() {
    // check status is maintained in own state
    const { checked } = this.state;
    let classNames = ['switch'];
    
    if (checked) {
      classNames = [...classNames, 'checked']
    }
    return (
      <div>
        <button className={classNames.join(' ')} onClick={this.toggle}/>
      </div>
    );
  }
}

class Wrapper extends React.Component {
  
  ref = React.createRef();

  getValue = () => {
    alert(this.ref.current.value);
  }
  
  render() {
    return <React.Fragment>
      <Switch ref={this.ref}/>
      <button onClick={this.getValue} style={{ marginTop: 20 }}> Switch Status </button>
    </React.Fragment>
  }
}

ReactDOM.render(
  <Wrapper />,
  document.getElementById('root')
);
```


## The Controlled

> A Controlled Component takes its current value through props and notifies changes through callbacks. A parent component "controls" it by handling the callback and managing its own state and passing the new values as props to the controlled component.

The following is an [example](https://codepen.io/n0rush/pen/jRreVJ) of a `Switch` implemented in controlled way.

```javascript
class Switch extends React.Component {
  constructor(props) {
    super(props);
  }
  
  toggle = () => {
    // callback passed in from parent
    const { checked, onChange } = this.props;
    onChange(!checked)
  }

  render() {
    // check status is maintained by props from parent component
    const { checked } = this.props;
    let classNames = ['switch'];
    
    if (checked) {
      classNames = [...classNames, 'checked']
    }
    return (
      <div>
        <button className={classNames.join(' ')} onClick={this.toggle}/>
      </div>
    );
  }
}

class Wrapper extends React.Component {
  
  state = {
    checked: false
  }

  getValue = () => {
    alert(this.state.checked);
  }
  
  // when check status changes, maintain it in parent state
  onChange = (checked) => {
    this.setState({
      checked
    })
  }
  
  render() {
    return <React.Fragment>
      <Switch checked={this.state.checked} onChange={this.onChange}/>
      <button onClick={this.getValue} style={{ marginTop: 20 }}> Switch Status </button>
    </React.Fragment>
  }
}

ReactDOM.render(
  <Wrapper />,
  document.getElementById('root')
);
```


## The Mixed

A `Mixed` Component allows for usage in either `Uncontrolled` or `Controlled` way. It accepts its initial value as a prop and puts it in state. It then reacts to props change through [Component Lifecycle](https://reactjs.org/docs/react-component.html) to sync state update to date with props.

[Switch Example in Mixed mode](https://codepen.io/n0rush/pen/NmrOao)

```javascript

class Switch extends React.Component {
  constructor(props) {
    super(props);
    // maintain check status in own state
    this.state = {
      checked: props.checked || false
    };
  }
  
  get value() {
    return this.state.checked;
  }
  
  static getDerivedStateFromProps(nextProps, currentState) {
    // react to props change and sync state accordingly
    // only in controlled mode
    if(nextProps.hasOwnProperty('checked') && (nextProps.checked != currentState.checked)) {
      return {
        checked: nextProps.checked
      }
    }
    return null;
  }
  
  toggle = () => {
    const { onChange, checked } = this.props;
    const { checked: checkedInState } = this.state
    if (!this.props.hasOwnProperty('checked')) {
      // no checked prop, uncontrolled
      this.setState({
        checked: !checkedInState
      })
    }
    onChange && onChange(!checkedInState)
  }

  render() {
    const { checked } = this.state;
    let classNames = ['switch'];
    
    if (checked) {
      classNames = [...classNames, 'checked']
    }
    return (
      <div>
        <button className={classNames.join(' ')} onClick={this.toggle}/>
      </div>
    );
  }
}

class Wrapper extends React.Component {
  
  ref = React.createRef();

  state = {
    controlledChecked: false,
  }

  controlledOnChange = (checked) => {
    this.setState({
      controlledChecked: checked;
    })
  }

  getUncontrolledValue = () => {
    alert(this.ref.current.value);
  }
  
  getControlledValue = () => {
    alert(this.state.controlledCheck);
  }
  
  render() {
    return <React.Fragment>
      <div>
        Uncontrolled: <Switch ref={this.ref}/>
        <button onClick={this.getUncontrolledValue} style={{marginTop: 20}}>
          Uncontrolled Switch Status 
        </button>
      </div>
      <hr/>
      <div>
        Controlled: <Switch checked={this.state.controlledChecked} onChange={this.controlledOnChange}/>
        <button onClick={this.getControlledValue} style={{marginTop: 20}}>
          Controlled Switch Status
        </button>
      </div>
      <hr/>
    </React.Fragment>
  }
}

ReactDOM.render(
  <Wrapper />,
  document.getElementById('root')
);
```

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.