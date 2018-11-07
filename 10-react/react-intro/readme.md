# Intro to React


## Framing
In unit 1 we build relatively complex apps in javascript.

Now we are going to see how javascript apps get built in the real world.

Just like in express and Rails, we are going to enforce some structure in the way that our javascript renders things in the DOM and also structure for the logic of our javascript as well.

We will be seeing some professional-level tools for running and storing our JS code.

React.js is a javascript library that basically tightly controls the rendering of HTML within the DOM.

Further, it is a javascript ecosystem for creating single page apps in javascript.

It's back-end equivalent would be more expresss than rails.

We will see that the professional react "stack" is quite complicated and includes some new tools and ways of working.

---
## What is reactjs
The react library itself is just a rendering layer to easily render elements onto an HTML page in the browser.

The wider react ecosystem allows you to build complex js single page applications.
---

### Top 3 React Whys:

1. Efficent and clean DOM rendering
2. Explicit data flow. No more global variables. Logic reacts as data flows.
3. Component library ecosystem.

---


## Hello World Example
Given we've included the right libraries:
```
<script src="https://unpkg.com/react@16/umd/react.development.js"></script>
<script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
<script src="https://unpkg.com/babel-standalone@6.15.0/babel.min.js"></script>
```

---

We have this main section:
```
<div id="root"></div>
<script type="text/babel">
ReactDOM.render(
  <p>hello world!</p>,
  document.getElementById('root')
);
</script>
```
---

## How does this work?
---

### 1. Babel
Babel is a *javascript transpiler*. It takes code and spits out new code. In fact, when we write react we are going to be working with an extension of the javascript language called JSX.

Babel is the tool we will use to *transpile* JSX to javascript.
---

#### 1.1 How Babel works
```
<script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.24/browser.js"></script>
<script>

   var reactJsx = `ReactDOM.render(
      <p>hello world!</p>,
      document.getElementById('root')
   );`;

  //var reactJsx = "ReactDOM.render(<p>hello world!</p>,document.getElementById('root'));";

  var result = babel.transform( reactJsx );

  console.log( result.code );
</script>
```

Inside of `result.code` is:
```
'use strict';

ReactDOM.render(React.createElement(
    'p',
    null,
    'hello world!'
), document.getElementById('root'));
```

---

Specifically Babel is looking for `<p>` and transforming it into `React.createElement`.

*But* the general purpose of babel is to take one set of javascript and transform (transpile) it into another version:
- JSX to ES5
- ES6 to ES5
- typescript to ES5
---


#### 1.2 Javascript `eval`
Javascript has the ability to execute code that is a string. (this is one of the features that makes it relatively insecure compared to languages that lack this feature)

```
var foo = "alert('hello world')";
eval(foo);
```

Babel takes the entire transpiled string and executes it for us.

---

### 2. ReactDOM
Now that we understand the magic behind this new `JSX` syntax, let's look at the example again.

Specifically:
```
ReactDOM.render
```

Using babel we created code that passed an element into ReactDOM.render.

When you take apart the above code it looks like this:
```
let element = React.createElement(
    'p',
    null,
    'hello world!'
);

ReactDOM.render(element, document.getElementById('root'));
```

Let's take out babel and the old code and paste that in.

---

## Rendering with Javascript

React and JSX are first of all just a HTML rendering layer for javascript.
```
const items = ["yay", "banana", "yellow"];

const list = items.map(item => {
  return <li>{item}</li>
});

let stuff = <ul>{list}</ul>

ReactDOM.render(
  stuff,
  document.getElementById('root')
);
```


Does this syntax look familiar? It works just like every rendering layer we've seen so far, in express and rails.

You can do anything you did in express and rails templates in JSX.
---

Conditional rendering:
```
let stuff = null;

if( user_logged_in ){
  stuff = <a>logout</a>;
}else{
  stuff = <a>login</a>;
}

ReactDOM.render(
  stuff,
  document.getElementById('root')
);
```
---

### Pairing Exercise: Render JSX with react and babel

1. Create a boilerplate HTML page.
```
touch index.html
```

```
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

  </head>
  <body>
  </body>
</html>
```


2. Include the libraries from the CDN:

```
<script src="https://unpkg.com/react@16/umd/react.development.js"></script>
<script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.24/browser.js"></script>
```

3. Add the `root` DOM element

```
<div id="root"></div>
```

4. In the javascript console:

Create some jsx and have Babel transpile it.

```
// The actual JSX code is: <p>hello world!!</p>
var reactJsxString = "ReactDOM.render(<p>hello world!</p>,document.getElementById('root'));";

var result = babel.transform( reactJsxString );

console.log( result.code );
```

5. Try JS eval on some random code

```
eval("alert('hello')");
```

6. Eval the generated code

```
eval(result.code);
```

##### Further
- What other attributes does `result` hold?
- Try building more complicated JSX and evaling it:
- Try the conditional and looping render.
- Change the DOM attachment point to something else. (Write other elements in the HTML)
- Put the jsx portion into a variable.
