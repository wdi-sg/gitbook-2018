# Dynamic Browser-Based TicTacToe

## Context:
How do we fit the model of while loops and waiting for user input into the model of `events`

#### Akira's reference implementation:
[https://github.com/awongh/tictactoe-textbased/blob/master/script.js](https://github.com/awongh/tictactoe-textbased/blob/master/script.js)

## Code-along: Changing the console game into a game that responds to events

```
<button id="row1-col1"></button>
<button id="row1-col2"></button>
<button id="row1-col3"></button>
<button id="row2-col1"></button>
<button id="row2-col2"></button>
<button id="row2-col3"></button>
<button id="row3-col1"></button>
<button id="row3-col2"></button>
<button id="row3-col3"></button>
```

```
// keep track of the current player
var currentPlayer = "X";

// set a callback function for when the user does a click

var userInput = function(){
  // get the current context of the click event
  console.log( this );

  // change the state of the board object
  https://github.com/awongh/tictactoe-textbased/blob/master/script.js#L62

  // change the dom to reflect the state of the board
    // use this keyword
    // change a value
};

// set the same function for each element of the board
var spaces = document.getElementByTagName('button');

for( var i=0; i<spaces.length; i++ ){
  spaces[i].addEventListener('click', userInput);
}
```

## Exercise:
Implement the code we just went through in class.
Use the above as starter code to make a tictactoe game that is playable.

#### Further:
- Use some nicer html than buttons to style your game. (`<div>` ?)
- Use images to style your game. Find and use images of x's and o's
