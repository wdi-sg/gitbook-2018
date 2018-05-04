# Bind for React Classes

## Click Handlers
First, let's see what a click handler looks like in react.
```
<button onClick={this.handleClick}>click me!</button>
```

Then we can write the handler in the class:
```
// our click method
handleClick(){
  console.log( "yay" );
}
```

This works just like normal js.

*But* what happens if we start doing stuff- for example we want to access things we did in normal javascript:

`this` refered to the button being clicked on.

If we `console.log` it, we can see that it's undefined. This also means we have no way of accessing the other attributes within the class.

Under the hood, what happens with any click handler (not just those in react) is that the window grabs the clikc handler for itself. That is why `this` in a click handler is the element being clicked on.

How do we set the context / value of this to the current class?

## Javascript Bind

##### `this` keyword: (review)
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

We need to bind our handleClick method to the current component.

### Exercise: JS bind
Start a new `index.html` file:

```
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
  </head>
  <body>
  <script>
  </script>
  </body>
</html>
```

In the script tag:
```js
var saySomething = {
  message : "hello",
  speak : function(){
    alert( this.message );
  }
};

saySomething.speak();
```

Call `saySomething.speak()` from the dev console to make sure it works.

Add a button into the body html
```html
<button id="submit">submit</button>
```

Set a click handler to the saySomething method.
```js
document.querySelector('#submit').addEventListener('click',saySomething.speak);
```

The message will be undefined.

See the return value of bind
```
saySomething.speak.bind( saySomething );
```

Fix it by doing a `bind` before you set the click handler.
```
saySomething.speak = saySomething.speak.bind( saySomething );
```

#### Further
Does this work instead? Why?
```js
document.querySelector('#submit').addEventListener('click',saySomething.speak.bind(saySomething));
```

#### Bind in react:
Run this code:
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Bind</title>
</head>
<body>

  <script src="https://unpkg.com/react@16/umd/react.development.js"></script>
  <script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
  <script src="https://unpkg.com/babel-standalone@6.15.0/babel.min.js"></script>

  <div id="root"></div>
  <script type="text/babel">

    class List extends React.Component {

        constructor(){
          super();
          this.yay = "banana";
        }

        // our click method
        handleClick(){
          console.log( "yay" );
          console.log( this.yay );
        }

        render() {
            return (
              <div>
                <button onClick={this.handleClick}>click me!</button>
                <ul>
                  <li>Hello world</li>
                </ul>
              </div>
            );
        }
    }

    ReactDOM.render(
        <List />,
        document.getElementById('root')
    );

  </script>

</body>
</html>
```
Add bind in the consturctor to make it work:
```
this.handleClick = this.handleClick.bind(this);
```
