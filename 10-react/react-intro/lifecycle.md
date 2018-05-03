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

### Create a form using the component lifecycle

The standard React way of dealing with forms is to set the value of the form using that component's state.

```
<input type="text" value={this.state.value} onChange={this.handleChange} />
```

