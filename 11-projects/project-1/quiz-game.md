# Project 1 - Plab B : Multiple Choice Quiz Game

Goal: Build a two-player multiple-choice quiz game!

## Requirements
* Gameplay Requirements
  * Your game should serve a (finite) number of questions
  * After answering a question, the DOM should be updated to show the next question
  * It should be able to check if the user's choice is correct
  * It should keep score for each player
  * It should alternate between the two players after each turn, and it should display who the current player is
  * It should be able to detect winner
  * It should have a reset button which restarts the quiz and resets the score (does not refresh the page)

* Bonus requirements:
  * Use of bootstrap
  * Mobile responsive design

## Specification
Writing programs according to a specification like this allows developers to
agree how programs will work before they're actually written. Here it allows us to
write tests separate of you writing the program. In the future program specifications
will be necessary to reach agreement on how things will work when working on
large projects with teams.

Define each question as an object, so that you can add key-value pairs each property of the question (e.g. the prompt, the options, and the index of the correct answer). This will be helpful in checking if the user's answer for each question is correct.

```
question1 = {
  prompt: "What is 10 + 10?",
  options: [10, 20, 30, 50],
  correctAnswerIndex: 1
}

question2 = {
  prompt: "Who is Moon Mayor?",
  options: ["Donald Trump", "Obama", "Steve Geluso", "Rachel Lim"],
  correctAnswerIndex: 2
}
```

Make the quiz an object too. This allows you to add key-value pairs each property of the quiz (e.g. what the questions are, what the current question is, what the scores are, etc.). This will be helpful in tracking the game play.
```
var quiz = {
  questions: [question1, question2], // question1 and question2 were defined above!
  isGameOver: false,
  currentQuestion: 0,
  player1Points: 0,
  player2Points: 0
}
```

## Test-driven development
Using [this test library](https://github.com/wdi-sg/quiz-tester) is optional but recommended. You may choose to ignore this and build your quiz game however you like.

We've built a test library that will check to see if your quiz runs correctly. In order
for it to work you'll need to format questions in your quiz exactly as described above.
You'll need to create a quiz object with properties exactly as shown above.

This script will test the game logic of your multiple choice quiz.
To use it you will need to include it in your html file after you main quiz script.
The application will console log all the passed or failed test.  

You will also need to declare the following functions in the global scope:

### numberOfQuestions()
It should return an integer that is the number of questions in a game

### currentQuestion()
It should return an integer that is the zero-based index of the current question in the quiz

### correctAnswer()
It should return an integer that is the zero-based index the correct answer for the current question

### numberOfAnswers()
It should return an integer that is the number of choices for the current question

### playTurn(choice)
It should take a single integer, which specifies which choice the current player wants to make.
It should return a boolean true/false if the answer is correct.

### isGameOver()
It should return a true or false if the quiz is over.

### whoWon()
It should return 0 if the game is not yet finished.
Else it should return either 1 or 2 depending on which player won.
It should return 3 if the game is a draw.

### restart()
It should restart the game so it can be played again.

## Running the Tests
The `quiz-tester.js` file is already added as a reference in your index.html file.
It will run every time the page is reloaded. See the results of the quiz tester by
opening your developer tools and looking at the console log.

You can disable running the tests by commenting out or removing the reference to
the `quiz-tester.js` script.
