# React Forms

Taking form input works much like the button in react. An event is set to an element and we render changes based on those events.

Let's change the button to a form input.

```
constructor(){
  super()
  this.changeHandler = this.changeHandler.bind( this );
}

this.state = {
  word : ""
}

changeHandler(event){
  this.setState({word:event.target.value});
  console.log("change", event.target.value);
}

render() {
    console.log("rendering");
    return (
      <div className="item">
        {this.state.word}
        <input onChange={this.changeHandler}/>
      </div>
    );
}
```

Now what if we want to control what the user sees in the input when they type?

We can update the input using `value`.

```
<input onChange={this.changeHandler} value={this.state.word.toUpperCase()}/>
```

This is using a pattern called [Controlled Components](https://reactjs.org/docs/forms.html).

We control what comes out of the form and what goes into `state`.

We listen to the value coming out of the form with `onChange` and we control the current value of the form with `value`.

We can also use it to validate an input.

```
changeHandler(event){
  if( event.target.value != 'a' ){
    this.setState({word:event.target.value});
  }
  console.log("change", event.target.value);
}
```

### Exercise
[https://github.com/wdi-sg/todo-react](https://github.com/wdi-sg/todo-react)
