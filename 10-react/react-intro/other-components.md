# Other Component Syntaxes

## Pure Functional Components

If your component does not have state, you do not actually need to write it with a class.

```
const HelloWorld = () => (<div>Hello world</div>);
```

```
const Notification = (props) => {
  const {level, message} = props;
  const classNames = ['alert', 'alert-' + level]
  return (
    <div className={classNames}>
      {message}
    </div>
  )
};
```

## Hooks
Hooks are a new way of doing state in react. They also enable some types functionality that before you would do with lifecycle methods.

```
class Example extends React.Component {
  constructor(){
    super()
    this.setCount = this.setCount.bind( this );
    this.state = {counter:0};
  }

  setCount(){
    this.setState({counter:0});
  }

  render() {

      console.log( "rendering");

      return (
        <p>You clicked {count} times</p>
        <button onClick={this.setCount}>
          Click me
        </button>
      );
  }
}
```

```
import React, { useState } from 'react';

function Example() {

  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);

  console.log( "rendering");

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

