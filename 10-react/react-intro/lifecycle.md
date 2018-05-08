# React Component Lifecycle

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
  console.log( "component should update");
  return true;

}

getSnapshotBeforeUpdate( prevProps, prevState ){
  console.log("get snapshot before update");
  return null;
}

componentDidUpdate( prevProps, prevState, snapshot ) {
  console.log( "component did update");
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

### Fetch
ES6 fetch replaces `XMLHttpRequest`. It will be very common to render a component and get data via ajax call. We time this using a lifecycle method.

Remember that we can never rely on the speed of an ajax request. Today if someone loads the app on their phone and they walk into a cell deadzone, it could take 30 seconds or more for that ajax request to complete.

Therefore, in the case that you are getting data when the component is rendered, (not on click), then the appropriate place to make the request is `componentDidMount`.

This is true because best practice is that if you are loading data, your component shows as "empty" until that data is downloaded.

You also cannot block react from rendering, so 99% of the time the render method will have been called before the request is finished anyways.

Example:
```
componentDidMount() {
  fetch('/api/pokemon/1')
    .then((response) => response.json())
    .then((myJson) => {
      this.setState({pokemon: [myJson, ...this.state.pokemon]});
    });
}
```
See [here.](https://github.com/wdi-sg/react-express-webpack/blob/fetch/src/client/components/counter/counter.jsx)

### Infinite Scroll
Another use of `componentDidMount` is making sure that the DOM is ready before we do some stuff.

We can make a general infinite scroll work by setting a scroll event on the entire window. When this event fires we can calculate if we need to request more stuff.

We have to put the event listeners in `componentDidMount` so that we are sure the DOM exists.

We also have to make sure to un-listen to the element in `componentsWillUnmount`. Otherwise this could cause memory leaks.
[https://github.com/wdi-sg/react-express-webpack/blob/scroll/src/client/components/items/items.jsx](https://github.com/wdi-sg/react-express-webpack/blob/scroll/src/client/components/items/items.jsx)

### ComponentDidCatch
Sometimes you have to deal with library or component errors that are somewhat out of your control.

In react, if you don't write code to explicitly deal with those errors, the whole application will crash.

React 16 gives us the ability to deal with those errors.

The classic example is an external npm package that gives errors for certain states. Esp. when making some api call that you need.

With componentDidCatch you can use logic to deal with the situation, such as retrying that component's request __x__ times before giving up.
```
componentDidCatch(error, info) {
  // Display fallback UI
  this.setState({hasError: true});
  // You can also log the error to an error reporting service
  console.log(error, info);
}
```
```
if( this.state.hasError === true ){
  message = <p className={styles.error}>Whoops, something went wrong!</p>;
}
```
