# React Render Timing / State

Now we have a button that can be clicked.

Let's change some attribute of the class, a counter that gets incremented.

```
constructor(){
  super()
  this.clickHandler = this.clickHandler.bind( this );
  this.counter = 0;
}

clickHandler(){
  console.log("clicking", this.counter);
  this.counter++;
}

render() {
    console.log("rendering");
    return (
      <div className="item">
        <button onClick={this.clickHandler}>YAY</button>
      </div>
    );
}
```

This code increments the value, but what happens when we try to output it?

```
<p>{this.counter}</p>
```

We can see that the class attribute gets incremented, but the screen doesn't change.

## React Rendering

Now we can talk about the real differentiation of the react library.

When we render something in react, the library is actually keeping track of something called a virtual DOM. When it sees changes it will call the appropriate `render` methods to output HTML.

The problem this is solving is that it turns out to be much faster to keep track of the DOM in javascript than it is *within the real DOM*.

The real work of building a react app is understanding this rendering cycle.

![https://docs.google.com/drawings/d/11ugBTwDkqn6p2n5Fkps1p3Elp8ZToIRzXzvM4LJMYaU/pub?w=543&h=229](https://docs.google.com/drawings/d/11ugBTwDkqn6p2n5Fkps1p3Elp8ZToIRzXzvM4LJMYaU/pub?w=543&h=229)

![https://calendar.perfplanet.com/wp-content/uploads/2013/12/vjeux/4.png](https://calendar.perfplanet.com/wp-content/uploads/2013/12/vjeux/4.png)

## React State

So we know about React properties, and how they relate to our component's data.
* The thing is, `props` represent data that will be the same every time our component is rendered. What about data in our application that may change depending on user action?
* That's where `state` comes in..

Values stored in a component's state are mutable attributes.
* Like properties, we can access state values using `this.state.val`
* Setting up and modifying state is not as straightforward as properties. It involves explicitly declaring the mutation, and then defining methods to define how to update our state...

If we want to trigger a rerender in our component then we have to keep track of an attribute in a specially named attribute called state.

Specifically if we want to trigger a rerender then we need to mutate that state with the method `setState`.

```js
class Item extends React.Component {

    //initialize the component
    constructor(){
      console.log("constructing");
      super()
      this.handleClick = this.handleClick.bind(this);
    }

    state = {
      counter : 0
    }

    // our click method
    handleClick(){
      let currentValue = this.state.counter + 1;

      console.log("clicking", currentValue);

      // set the state of this component
      this.setState( { counter: currentValue } );
    }

    // what happens when the component renders
    render() {
        console.log("rendering");
        return (
          <div>
            <span>{this.state.counter}</span>
            <button onClick={this.handleClick}>click me!</button>
          </div>
        );
    }
}

ReactDOM.render(
    <Item />,
    document.getElementById('root')
);
```

### Props vs. State
- props don't change within the component.
- props "come from above" (or are set in the constructor)
- state is changed within the component itself

### Re-rendering with props
If we have a sub component that takes in a changing prop, that component also gets rerendered:
Let's put our span inside it's own component:
```
<span>{this.state.counter}</span>
```
changes into:
```
<Count counter={this.state.counter}/>
```
```
class Count extends React.Component {

    render() {
        console.log("rendering count component");
        return (
          <div>
            <span>{this.props.counter}</span>
          </div>
        );
    }
}
```

### Exercise
Build the above counter increment.

Watch the console to see when clicking and rendering happen.

Don't forget to include the 3 libraries you need:
```
<script src="https://unpkg.com/react@16/umd/react.development.js"></script>
<script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
```

#### Further
Build the counter display into it's own component.

#### Further
Make a second and third button that increments by 2 and 3. Send those props to the display component.

Display the current count and an array of previous values in the display component.
