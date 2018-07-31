# React Router

## Learning Objectives

* Review importing and using third-party node modules into React using npm
* Use React Router's `BrowserRouter`, `Link`, `Route`, `Redirect`, and `Switch` components to add navigation to a React application
* Review the React component lifecycle and use component methods to time API calls

## Framing (5 min / 0:05)

Up to this point, our React applications have been limited in size, allowing us to use basic control flow in our components' render methods to determine what gets rendered to our users. However, as our React applications grow in size and scope, we need an easier and more robust way of rendering different components. Additionally, we will want the ability to set information in the url parameters to make it easier for users to identify where they are in the application.

React Router, while not the only, is the most commonly-used routing library for React. It is relatively straightforward to configure and integrates with the component architecture nicely (itself being nothing but a collection of components). Once configured, it essentially serves as the root component in a React application and renders other application components within itself depending on the path in the url.

## [React Webpack Boilerplate](https://github.com/wdi-sg/react-express-webpack) Setup (5 min / 0:10)

## React Router Setup (10 min / 0:40)

### Importing Dependencies

First, we need to install `react-router-dom` and save it as a dependency to `package.json`...

```sh
npm install --save react-router-dom
```

To configure our current application to use React Router, we need to modify the root rendering of our app in `index.js`. We need to bring in the `Router` component and allow it to be the root component of our application. `Router` will, in turn, render `App` through which all the rest of our components will be rendered:

```js
// index.js

import { BrowserRouter } from "react-router-dom";

// ...

ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  document.getElementById("app")
);
```

By making `BrowserRouter` the root component of our app, all child components, including `App` will have access to a `history` object through which information like the current location and url can be accessed or changed. Additionally, in order to use the other routing components provided by React Router, a `Router` ancestor component is necessary.

Next, in `App.js`, we need to import all of the other components we want to use from React Router...

```js
// src/components/App/App.js

import { Route, Link } from "react-router-dom";
```

### Modifying App's render method

Now that we have access to these components, we need to modify the `App` component's `render()` method to set up navigation. The basic structure we will use is...

```js
// src/components/App.jsx

render() {
  return(
    <div>
      <nav>
        <h1>React Router</h1>
        <Link to="/bananas">Bananas</Link>
        <Link to="/oranges">Oranges</Link>
      </nav>
      <main>
        <Route
          path='/bananas'
          render={() => (
            <Bananas/>
          )}
        />
        <Route
          path='/oranges'
          render={() => (
            <Oranges/>
          )}
        />
      </main>
    </div>
  )
}
```
> **Link** - a component for setting the URL and providing navigation between different components in our app without triggering a page refresh. It takes a `to` property, which sets the URL to whatever path is defined within it. Link can also be used inside of any component that is connected to a `Route`.

> **Route** - a component that connects a certain `path` in the URL with the relevant component to `render` at that location (similar to `body` from handlebars).

> Notice that we are writing an anonymous callback function in the value for the `Route`'s render property. This callback function **must return a React component**.

> So long as we use an ES6 arrow function, this callback will preserve context (i.e. the value of `this`), allowing us to pass down data and functions into `Search` from `App`. You can use an ES5 anonymous function, but you will then need to use `.bind()` to preserve context.

See more about the route component [here.](https://reacttraining.com/react-router/web/example/url-params)

## Redirecting (15 min / 1:10)
Sometimes you want to change the path of the app in response to the actions that happen within the app.

If you want to change the path after getting and displaying some info (as opposed to rendering a component from a top level router/link) you can do that by manually __pushing__ into the browser history.

This utility is given to you as a library that is used within react router.

```
import React from 'react';
import { withRouter } from 'react-router-dom';

class Bananas extends React.Component {
  constructor(){
    super();
    this.clickHandler = this.clickHandler.bind(this);
  }

  clickHandler(){
    setTimeout(()=>{
      this.props.history.push('/results');
    },1000);
  }

  render() {
    return (
      <div>
        <p>Bananas</p>
        <button onClick={this.clickHandler}>Click Me!</button>
      </div>
    );
  }
}

export default withRouter(Bananas);
```

We're using a setTimeout again to simulate an ajax request.

When the "request" is done, we can change the current path of the browser.

Note that the back button of the browser still works the way we have set things up.

Also note that we are including react router at the top of the file and using it at the bottom with `export default withRouter(Bananas);`

From the react router docs:
```
You can get access to the history object's properties and the closest <Route>'s match via the withRouter higher-order component. withRouter will pass updated match, location, and history props to the wrapped component whenever it renders.
```

## See Also:
[React Router Website Docs](https://reacttraining.com/react-router/web/guides/philosophy)
[React Router Repo Docs](https://github.com/ReactTraining/react-router/tree/master/packages/react-router/docs)

## Exercise
Run some code with react router:

Clone another copy of [react-express-webpack](https://github.com/wdi-sg/react-express-webpack)

Install the react router npm package:
```
npm install --save react-router-dom
```

Copy the above code from `App.jsx` and `index.jsx`.

You can create and `import` the `oranges` and `bananas` components, or you can use the `form` and `counter` components that are already there.

Make sure to do a lot of console.logs in the render to understand what is happening.

Here is a working [solution.](https://github.com/wdi-sg/react-express-webpack/tree/router)

If you want to see another `history.push` example, here is one that [increments infinite scroll pages.](https://github.com/wdi-sg/react-express-webpack/tree/router-history-push)
