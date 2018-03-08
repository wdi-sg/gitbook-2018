## Coding Approaches Review
![https://d.justpo.st/media/images/2013/03/463e4e4f4acb9afc185a067b7f360321.jpg](https://d.justpo.st/media/images/2013/03/463e4e4f4acb9afc185a067b7f360321.jpg)

* Break your problems down into smaller problems

    * Corrolary 1: Don't code too much right away. Running code is a leg to stand on. It is easier to work forward from there.
      * Sub-corrolary: Did you get something done? Maybe now is a good time for a commit.

    * Corrolary 2: If you are stumped, solve some small problem right now, that you know is solvable. (Even if it doesn't seem immidiately related to the problem)

* Constantly formulate and test your hypotheses about what your code is doing
  * Example: I know that *x* but I think maybe *y* ... test *y* ... repeat ...
  * Note: use your debugging tools to test those hypotheses: debug console, console.log

* For abstract word problems, try to recognize what the right tool is. Is it a conditional? A loop? An array? An object? These things will develop with practice.

* If you are stuck on a hard problems, start with pseudocode
* If you don't know where to start, start with pseudocode

* If you are stuck on an error look up and down the stack: did you misspell something? Did you save the file? Are you missing a semicolon? Is your computer on?
* Carefully read your error messages. With experience you will know which messages are generic and whcih ones are insightful, but even the generic ones can tell you something. ( `x is undefined` )

* Write comments for each line of code:
  * As a guide to other programmers
  * To rememebr what you were thinking there

* Try to only do one thing per line of code

```
  createHouse( createRoom( calculateRoomSize( 23 + currentRoomSize ) ) );
```

* If your code **looks** too long, is too nested, is confusing for you to look at, is not commentable, you should take a moment to refactor __when it works__

* code smell is important
[https://techbeacon.com/35-bad-programming-habits-make-your-code-smell](https://techbeacon.com/35-bad-programming-habits-make-your-code-smell)

* Your mental and virtual environment is very important in helping you focus on the problem at hand
[http://heeris.id.au/trinkets/ProgrammerInterrupted.png](http://heeris.id.au/trinkets/ProgrammerInterrupted.png)
  * decide on a way to organize your files
  * don't keep too many things open on your environment
  * separate things for yourself to keep track of them
