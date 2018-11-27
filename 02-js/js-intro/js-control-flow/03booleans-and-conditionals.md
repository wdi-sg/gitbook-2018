# Intro to Conditionals

![](https://assets.hongkiat.com/uploads/programming-jokes/joke-programmers-butters.jpg)

### Lesson Objectives
_After this lesson, students will be able to:_

- Explain why we would use 'control flow' in our programs
- Write a simple if statement
- Write an if / else statement

<br>
<hr>

&#x1F535; **Setup**
You will be using the file called `conditional_practice.js` in your `wdi` folder to test your code.

# Control Flow

Control Flow is the order in which individual statements or instructions are executed.

:door: **Example**

You want to enter a house. You have to walk through the door to get inside. Based on whether that front door is open or closed, locked or unlocked, you may have to take a different step to get inside the house.

Control Flow lets you specify **when** and **which** lines of code get executed

:door: **Example**

- When the door is open: I walk inside.

- When the door is closed:
I determine if it's locked.

There are three forms of Control Flow:

- **Conditionals** - skip lines of code
- **Loops** - repeat lines of code
- **Functions** - save and later deploy lines of code

When writing a program, each of these three are the techniques we use to define the exact intention that we are trying to express.

If we aren't explicit enough (see above) then the output of our programs won't be correct.

# Conditionals

We can set up a branching tree-like structure and control the flow of our code. Though, our code will look less like a tree and more like:
```
if (BOOLEAN EXPRESSION){
  // run this code
};
```
If the boolean expression within our condition is `true`, a branch will execute. If it is `false`, it will not execute. This is an example of `control flow`.

![control flow wd40](https://i.imgur.com/v4W1xwD.png)

Before we write in some control flow, let's take a closer look at the boolean logic that will drive our conditionals.

<br>
<hr>

# Booleans

* Booleans allow us to set `true` and `false` values. With true and false values, we can control the flow of our programs.

:door: **Example**

The door to our house from earlier, we could create a variable for `door_open` and its value can be set to either true or false. If it's true, we can run a line of code to walk through the door. If the `door_open` variable is false, then we would have to run a different line of code to figure out what to do next.

:books: **History Lesson**

 Boolean logic is a type of logic that deals with binary values, and is named after George Boole who invented it.

<hr>

### Let's Practice :computer:

1) Using your terminal, create a file called `conditional_practice.js` link it to the `index.html` we created before.

Note: all activities for this morning will be done in this `conditional_practice.js` file.

:elephant: **Reminder** <br>
Use `cd` to change directories and `subl filename` to open a file with Sublime.

2) Open the file's folder in finder and click on the `index.html` to open it in chrome.

3) Let's declare some variables with Boolean values.

```
var sunny = true;
var notSunny = false;
```

4) Check the values of your variables by logging them and running your code.

:elephant: **Reminder** <br>
- To log your code:
`console.log(variable_name or "message")`
<br>

<hr>

## Not
- `!` **not** sometimes called a 'bang': changes Boolean value to its opposite.

### Let's Practice :computer:

Uh oh! There are cloudy skies :cloud::cloud::cloud:

1) Change the value of your `sunny` variable to `!true`. Log `sunny` and run your code to see the result.

2) Using a bang, change the value of `notSunny` to result in true.

3) Add the following code to your file to test out:

```
let toggle = true;
console.log("this is toggle " + toggle);
toggle = !toggle;
console.log("this is toggle " + toggle);
```
**Look at that!** :eyes: <br>
You can reassign values with a variable's own value (a little bit of a brain bender)!

<hr>

## Equality operators
Equality operators are a boolean expression that determines if one thing is equal to another *or not*.
Like all boolean expressions it evaluates to either true or false.

`==`, `!=`, `===`, `!==`

Equality operators are **not** the same as the _assignment_ operator `=`

- `==` **equality**: compares left-hand and right-hand and checks if they are the same. Returns a Boolean value.
- `!=` **inequality**: compares left-hand and right-hand and check if they are not the same. Returns a Boolean value.

```
true == true
=> true
```

```
true == false
=> false
```

&#x1F535; **Examples**

Booleans:

```
false == false
=> true
```

```
true != true;
=> false
```

```
true == !true
=> false
```

Numbers:

```
1 == 1
=> true
```

And with strings:

```
"hello world" == "hello world"
=> true
```

### Strict

- `===` **strict equality**: same as equality, but does not coerce
- `!==` **strict inequality**: same as inequality, but does not coerce

```
5 == '5';
=> true
```

```
5 === '5';
=> false
```

### Let's Practice :computer:

1) Is the number 1 equivalent to the number 1?

2) Is the string "beans" equivalent to the string "soup"?

3) Is (5 + 5 * 3) equivalent to ((5 + 5) * 3)?

4) Is 9 strictly unequal to false?

<hr>

## Truthiness

**All** values in JavaScript have an implicit 'truthiness'. They can be evaluated as either true or false. In effect, every value in Javascript is converted into a Boolean when it is checked as an expression of truth.

##### All of the following become false when converted to a Boolean

- `false`
- `0`
- `""` (empty string)
- `NaN`
- `null`
- `undefined`

<br>

### Let's Practice :computer:

JavaScript has a built-way to convert things to Booleans: `Boolean()`. Put the following inside the parenthesis of `console.log()` to see the result.

```
Boolean("");
Boolean(null);
Boolean(0);
```
<br>


##### All other values are implicitly true

### Let's Practice :computer:

```
Boolean("hi");
Boolean(1);
Boolean(true);
```

<br>
<hr>




## Comparison Operators

[Comparisons](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comparison_Operators) in JavaScript can be made using `<`, `>`, `<=` and `>=`. These work for both strings and numbers. This is both useful, and can be the source of frustration for some developers, since most languages do not implicitly convert strings to numbers the way that JavaScript does.

```javascript
"A" > "a"
//=> false

"b" > "a"
//=> true

12 > "12"
//=> false

12 >= "12"
//=> true
```

```
5 > 2;
=> true
```

```
5 <= 5;
=> true
```

- strings are compared by alphabetical precedence

```
`'a' > 'b'`;
=> false
```

```
`'z' > 'abc'`
=> true
```


### Let's Practice :computer:

1) Is -10 greater than or equal to -100?

2) Is Infinity greater than or equal to -Infinity?

3) Is "McDonald's" greater than "Burger King?"

<br>
<hr>

## Logical operators

`&&`, `||`

- `&&` **and**: evaluates to `true` if both sides are true. It does not check for equality! Rather, **and** evaluates the truth of the statement, and will return the value of one of the operands.

```
true && true
=> true
```

```
false && false
=> false
```

In these examples, each side is the same (they are equivalent), but in this case, both sides do not both evaluate to `true`.
If an `&&` statement begins with `false`, it automatically evaluates to false.
Both sides must evaluate to true to && to result in true.

```
true && false
=> false
```

```
var a = true;
var b = false;

a && b
=> false
```

<br>

- `||` **or**: evaluates to true if _either_ side is true.

```
true || false
=> true
```

```
false || false
=> false
```

```
var x = false
var y = false

x || y
=> false
```


### Boolean order of evaluation

0. `()`
1. `>, <, >=, <=`
2. `==, ===`
3. `&&`
4. `||`

<br>

### Let's Practice :computer:

Try to guess the result before you check it. If it is not what you expected, try to find out why not

* Check: `!false && true`
* Check: `false || true`

```
var a = true;
var b = false;
```
* Check: `a && a == b`
* Check: `!true || !false && !false`
* Check: `8 > 1 && true || false`

<br>
<hr>

## IF Statements

Basic if statement

```
if (BOOLEAN EXPRESSION) {
  // run this code
}
```

The curly braces denote a `block`. The `block` will run if the `BOOLEAN EXPRESSION` evaluates to `true`.

**Example**

```
var number = 10;

if (number === 10){
  console.log("The number is 10!")
}
```

### Let's Practice Together :computer:

#### Reporting for Duty
_Strings as conditionals_

1) Make a variable called `name` and save a name to it.

3) Log the variable to confirm what you've stored.<br>
`console.log(name);`

4)Add a basic **if statement** to add control flow depending on the input.

```
if (name === "Kermit") {
  console.log("Hi ho! Kermit the frog here.");
}
```

- If the input name is `Kermit`, the code is run. Otherwise, the code never runs.

- Control flow with conditionals means that not every line is run. The code in front of us is separate from the process going on behind the scenes.

- Lines of code will be excluded during execution in order to take us on a particular 'branch'

- Which lines are excluded depends on Boolean values, and whether expressions evaluate to `true` or `false`

<br>
<hr>

#### Have a Drink
_Numbers as conditionals_

```
var age = 21;

if (age >= 21) {
    console.log ('You are allowed to buy beer');
}
```

#### Oddball
_Modulus as conditionals_

`%`

Check for odd numbers:
```
if (5 % 2 == 0) {
    console.log('number is even);
}
```

**Let's add an `else` to our `if` statement**

### If / else

```
if (5 % 2 ==0) {
    console.log('Number is even');
} else {
    console.log('Number is odd');
}
```

### Pairing Exercise:

#### Part 1: Operators

Paste the above examples into the chrome dev console. See what the output values are. Change the operators for each example or change the values to see what happens.

#### Part 2: Conditional `if` `else` `if else` Statements

Create a new html file: `touch conditionals.html`

Create a new js file: `touch conditionals.js`

Link them together in your HTML file: `<script src="conditionals.js"></script>`

Paste the above conditional statement examples into your js file one at a time.

Use `console.log` to see what values you are getting.

Also try playing with the examples in the chrome console.

#### Further Exercises:

Starting at the bottom of your js file, create a variable `speed`. write the contitional for a traffic stop. If speed is less than 10 `console.log` "I pulled you over because you were going too slow". If speed is more than 50 `console.log` "I pulled you over for going to fast".

Create a variable `tirePressure`. If tire pressure is less than 10 PSI `console.log` "I pulled you over because you are driving with a flat tire".

Create a variable `driverVision`. Assign an array value: `[6,6]`. If `driverVision` is less than 6/12, `console.log` "Sorry you can't drive".

Now, write some more complicated conditional logic:

If driverVision is over 6/6 set the speed variable to 60.

If speed is over 50 and tirePressure is under 10 or over 100 `console.log` "car crash".

If speed was under 10 and tirePressure was over 100 `console.log` "rolling to a stop".

If driverVision is over 6/12 and speed is over 50 `console.log` "car crash".

If the car will crash, don't output the traffic stop text.
