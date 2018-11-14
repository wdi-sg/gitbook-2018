# React Component Lifecycle

![https://github.com/wdi-sg/gitbook-2018/blob/8001ca2ffc95d1bc20152ca197f3fd808f06491f/react-lifecycle.jpeg?raw=true](https://github.com/wdi-sg/gitbook-2018/blob/8001ca2ffc95d1bc20152ca197f3fd808f06491f/react-lifecycle.jpeg?raw=true)

What is actually happening when we update the counter?

```
class ListItem extends React.Component {

    render() {
        return (
          <li>{this.props.item}</li>
        );
    }
}



class List extends React.Component {
    constructor (props) {
      super()
      this.state = {
        counter: 0
      }
    }

    handleClick(e) {
      this.setState({
        counter: this.state.counter + 1
      })

    }

    render() {
        let itemsElements = this.props.items.map(item => {
                              return <ListItem item={item}></ListItem>;
                            });
        return (
          <div>
            <p>Count is: {this.state.counter}</p>
            <button onClick={(e) => this.handleClick(e)}>click me!</button>
            <ul>
              {itemsElements}
            </ul>
          </div>
        );
    }
}

const listOfItems = [
  "apples",
  "bananas",
  "pineapple"
];

ReactDOM.render(
    <List items={listOfItems} />,
    document.getElementById('root')
)
```


Add our lifecycle methods into the `listItem` component
```
constructor(){
  super();
  console.log("constructor");
  this.state = {};
}

componentDidMount() {
  console.log( "component did mount");
}

static getDerivedStateFromProps( nextProps, prevState ){

  console.log( "get derived state from props");
  return null;
}

shouldComponentUpdate( nextProps, nextState ) {
  console.log( "decide if component should update");
  return true;

}

getSnapshotBeforeUpdate( prevProps, prevState ){
  console.log("get snapshot before update");
  return "***the snapshot***";
}

componentDidUpdate( prevProps, prevState, snapshot ) {
  console.log( "component did update");
  console.log( "snapshot from get snapshot before update: "+snapshot);
}

componentWillUnmount() {
  console.log( "component will unmount");
}
```

### Order of Events:

When we reload the page and see the code run we get:

- constructor
- get derived state from props
- render
- component did mount

Then when we click the button:
- get derived state from props
- component should update
- render
- get snapshot before update
- component did update

### React v16 Lifecycle Updates (27/3/2018)
React is getting rid of some lifecycle methods that you may see in other documentation or tutorials.

This is being considered as "extra" -> most functionality that you see can be moved into the constructor.

`componentWillMount` ==> `constructor`

`componentWillReceiveProps` ==> `static getDerivedStateFromProps`

`componentWillUpdate` ==> `getSnapshotBeforeUpdate`


### Exercise:
Run the above code.

## What situations would you use lifecycle methods?
Lifecycle methods are there for you to do things outside the normal react props/state rendering flow.

In react speak, it is when your app has "side-effects", or does something outside of just setting state or passing props.

Let's look as some specific examples.

### Ajax

Remember that we can never rely on the speed of an ajax request. Today if someone loads the app on their phone and they walk into a cell deadzone, it could take 30 seconds or more for that ajax request to complete.

Therefore, in the case that you are getting data when the component is rendered, (not on click), then the appropriate place to make the request is `componentDidMount`.

This is true because best practice is that if you are loading data, your component shows as "empty" until that data is downloaded.

You also cannot block react from rendering, so 99% of the time the render method will have been called before the request is finished anyways.

Example:
```
componentDidMount() {
  var responseHandler = function() {
      reactThis.setState({stuff: this.responseText});
  };

  var request = new XMLHttpRequest();

  request.addEventListener("load", responseHandler);

  request.open("GET", "https://swapi.co/api/people/1");

  request.send();
}
```
See [here.](https://github.com/wdi-sg/react-express-webpack/blob/fetch/src/client/components/counter/counter.jsx)

For more info and specific use cases see: [https://reactjs.org/docs/react-component.html](https://reactjs.org/docs/react-component.html)

### Infinite Scroll
Another use of `componentDidMount` is making sure that the DOM is ready before we do some stuff.

We can make a general infinite scroll work by setting a scroll event on the entire window. When this event fires we can calculate if we need to request more stuff.

We have to put the event listeners in `componentDidMount` so that we are sure the DOM exists.

We also have to make sure to un-listen to the element in `componentsWillUnmount`. Otherwise this could cause memory leaks.
[https://github.com/wdi-sg/react-express-webpack/blob/scroll/src/client/components/items/items.jsx](https://github.com/wdi-sg/react-express-webpack/blob/scroll/src/client/components/items/items.jsx)

### Side Effect Components
Sometimes a library asks you to bind to an element in the DOM.

Because it happens outside of react this is called a "side-effect".

`SearchBox` takes a DOM `inputElement` and sets it's own listeners on it.

```
var searchBox = new google.maps.places.SearchBox(inputElement);

searchBox.addListener('places_changed', changePlaceHandler);
```

If react rerenders this element, we will lose it's state and context.

### ComponentDidCatch
Sometimes you have to deal with library or component errors that are somewhat out of your control.

In react, if you don't write code to explicitly deal with those errors, the whole application will crash.

React 16 gives us the ability to deal with those errors.

The classic example is an external npm package that gives errors for certain states.

With componentDidCatch you can use logic to deal with the situation.

More on error boundaries: [https://medium.com/@sgroff04/2-minutes-to-learn-react-16s-componentdidcatch-lifecycle-method-d1a69a1f753](https://medium.com/@sgroff04/2-minutes-to-learn-react-16s-componentdidcatch-lifecycle-method-d1a69a1f753)
```
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      error: null,
      errorInfo: null
    };
  }

  componentDidCatch(error, errorInfo) {
    // Catch errors in any child components and re-renders with an error message
    this.setState({
      error: error,
      errorInfo: errorInfo
    });
  }

  render() {
    if (this.state.error) {
      // Fallback UI if an error occurs
      return (
        <div>
          <h2>{"Oh-no! Something went wrong"}</h2>
          <p className="red">
            {this.state.error && this.state.error.toString()}
          </p>
          <div>{"Component Stack Error Details: "}</div>
          <p className="red">{this.state.errorInfo.componentStack}</p>
        </div>
      );
    }
    // component normally just renders children
    return this.props.children;
  }
}

class BuggyButton extends React.Component {
  constructor(props) {
    super(props);
  }

  render() {
  	this.props.flaky();
    return (
      <button className="btn" onClick={this.handleClick}>
        {"Scary Button!"}
      </button>
    );
  }
}

function App() {
  return (
    <ErrorBoundary>
      <BuggyButton />
    </ErrorBoundary>
  );
}

ReactDOM.render(<App />, document.getElementById("root"));
```
