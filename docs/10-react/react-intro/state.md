# React State

So we know about React properties, and how they relate to our component's data.
* The thing is, `props` represent data that will be the same every time our component is rendered. What about data in our application that may change depending on user action?
* That's where `state` comes in..

Values stored in a component's state are mutable attributes.
* Like properties, we can access state values using `this.state.val`
* Setting up and modifying state is not as straightforward as properties. It involves explicitly declaring the mutation, and then defining methods to define how to update our state...

Lets implement state with a counter.
```js
class Item extends React.Component {

    //initialize the component
    constructor(){
      super()
      this.handleClick = this.handleClick.bind(this);
    }

    // set any kind of variable on this class
    state = {
      counter : 0
    }

    // our click method
    handleClick(){
      let currentValue = this.state.counter + 1;

      // set the state of this component
      this.setState( { counter: currentValue } );
    }

    // what happens when the component renders
    render() {
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

## Render timing in React
Let's talk about the step by step process that react has for rendering a component on the screen.

The magic of react is that it doesn't render anything unless it has to.

Under the hood it is keeping track of all changes and calculating what to change with a "virtual DOM". It's doing this because it will optimize itself to render only the parts of the DOM it needs to, when it needs to.

What does this mean for the rendering process in react?

Rendering happens when 1 of 2 things occurs:
1. The component recieves a new state from it's parent component.
1. The component changes it's internal `state` object.

Even if we aren't displaying anything new to the user, as long as some data (props or state) in the component has changed, react will re-render the component. (That component's `render` method will be called.)

Let's demonstrate that now:

Add a `console.log` to each method in our item component.

Run your app again to see what is happening. (refresh the page)

Add three new components to our file.
Make the inside of them the same as `Item`.
Change the console logs to reflect the name of the component.
```js
class Banana extends React.Component {
```

```js
class Kiwi extends React.Component {
```

```js
class Orange extends React.Component {
```

Render the components in the `Item` render method.
```js
render() {
    console.log('rendering Item component');
    return (
      <div>
        <span>{this.state.counter}</span>
        <button onClick={this.handleClick}>click me!</button>
        <Banana foobar={this.state.counter} />
        <Kiwi foobar={this.state.counter} />
      </div>
    );
}
```

Change the Banana component to render an `Orange` component
```js
render() {
    console.log('rendering Banana component');
    return (
      <div>
        <span>{this.state.counter}</span>
        <button onClick={this.handleClick}>click me!</button>
        <Orange foobar={this.state.counter} />
      </div>
    );
}
```

Run your app again to see what is happening when.
Click some of the buttons to see the order of events.
Render the props being passed in:
```
<p>{this.props.foobar}</p>
```

### Props vs. State
- props don't change
- props "come from above" (or are set in the constructor)
- state is changed within the component itself

## Javascript Bind

##### `this` keyword:
In javascript we use `this` keyowrd to refer to the current context of the function.

Given this ES5 JS:
```js
var saySomething = {
  message : "hello",
  speak : function(){
    alert( this.message );
  }
};

saySomething.speak();
```

When we look at `this.message`, it refers to the message key *inside the current object*.

Given this code:

```html
<button id="submit">submit</button>
```

```js
document.querySelector('#submit').addEventListener('click',function(){
  console.log(this);
});
```

`this` keyword refers to the button being clicked, or, the current context of execution.

#### JS Execution Context

What would happen if we used our speak method as a click callback?
```js
document.querySelector('#submit').addEventListener('click',saySomething.speak);
```

What is actually happening is that when you set a callback is that the DOM holds onto your callback function- *the context of the execution* changes.

#### Why we need bind:
With `bind` javascript gives us an explicit way to set the function context before the function gets executed.

```js
saySomething.speak = saySomething.speak.bind( saySomething );
```

From [MDN:](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_objects/Function/bind)
> The bind() method creates a new function that, when called, has its this keyword set to the provided value, with a given sequence of arguments preceding any provided when the new function is called.

When we say:
```html
<button onClick={this.handleClick}>click me!</button>
```

React isn't doing any magic to fix the above problem.

We need to bind our handleClick method to the current component, or it will never be able to do anything like `setState`.
