# React Forms

Taking form input works much like the button in react. An event is set to an element and we render changes based on those events.

Let's change the button to a form input.

```
constructor(){
  super()
  this.changeHandler = this.changeHandler.bind( this );
}

state = {
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
// this form input will set state on anything except 'a'
changeHandler(event){
  if( event.target.value != 'a' ){
    this.setState({word:event.target.value});
  }
  console.log("change", event.target.value);
}
```

### Exercise
Implement the above form in react.

Don't forget to use `http-server`

Don't forget to include the required libraries:

```
<script src="https://unpkg.com/react@16/umd/react.development.js"></script>
<script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
<script src="https://unpkg.com/babel-standalone@6.15.0/babel.min.js"></script>
```

#### further
Create validation on the form. Use the react `className` to set a css class on the form that tells the user that their input is invalid.

Example: User's input can't be longer than 6 characters. After 6 characters, apply the `warning` class to the input.

```
.warning{
  border 1px solid red;
}
```

#### further
When the form input reaches 5 characters, change the input value to all capital letters.

#### further
Change the form to never display the letters the user types in the input. Instead, display them in an `h1` tag.
