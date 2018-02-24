# Booleans & Conditionals

## Learning Objectives
- Differentiate between `true` & `false` && "truthy" & "falsey"
- Write an `if`-`else if`-`else` statement in JS and describe how control flow is used in programming

## Framing
- booleans and conditionals are what separates "procedural" languages from "turing complete" languages like javascript. All programming languages you will encounter have some kind of conditional statement.


## Booleans

A boolean can have one of two values: `true` or `false`. These are not strings -- they are a unique data type that represent the concepts of true and false.

## Comparison Operators

We encounter booleans most often when comparing two values using comparison operators like...
* `==` - equal (in value)
* `===` - equal (in value and data type)
* `!=` - not equal (in value)
* `!==` - not equal (in value and data type)
* `<` - less than
* `>` - greater than
* `<=` - less than or equal to
* `>=` - greater than or equal to

What is the difference between `==` and `===`?
* `===` checks for both the data type and value.
* `==` only checks for value. Under the hood, though, `==` converts the data type to the same data type and then executes comparison.

```js
1 == 1
// Does 1 equal 1? Yes ==> true

1 == 2
// Does 1 equal 2? No ==> false

1 === 1
// Does 1 equal 1 AND are both values numbers? Yes ==> true

1 == "1"
// Does 1 equal 1? Yes ==> true
// In this scenario, the numerical value inside "1" is evaluated. Javascript doesn't care that the right side is a string because we are using `==`

1 === "1"
// Does 1 equal 1 AND are both values numbers? No ==> false
// This time, Javascript does care about data types because we are using `===`. Because the left side is a Number and the right side is a String, this evaluates to false.
```

The assignment operator `=` is not the same as `==` or `===`!
* We use `=` when assigning a value to a variable
* We use `==` and `===` when comparing two values

> Make sure never to use `=` inside of a conditional statement!

## Truthy vs. Falsey

There is also a concept of "truthy" and "falsey," meaning that when certain things are evaluated in a conditional (more on those later in this document) they will be evaluted as either `true` or `false`.

The following things are falsey...
- `false`
- `0` (zero)
- `""` (empty string)
- `null`
- `undefined`
- `NaN` (a special Number value that stands for "Not A Number")

Everything else is "truthy".

> We will revisit this concept below in the "Conditionals" section.

## Logical Operators

Logical operators allow us to compare two or more values that evaluate to booleans.

- `&&` - AND
- `||` - OR
- `!` - NOT

Run the following lines one-by-one in the browser console. What do they evaluate to?

```js
true && false
// False. `&&` only evaluates true when the values on both sides of the operator evaluate to true. In this case, the right side is false.

true || false
// True. `||` evaluates as true if one of the values its assessing is true. In this case, the true on the right side is enough for the overall statement to evaluate as true.

5 > 12 && 12 >= 12
// False. `5 > 12` evaluates to false. If one thing on either side of `&&` is false, the entire statement evalutes to false.

17 > 12 || 4 <= 4
// True. This time both sides of `||` evaluate to true, meaning the whole statement evaluates to true.

!(true)
// False. ! will give you the opposite boolean value of whatever it precedes. In this case, the opposite of true is `false`.

!(5 > 12)
// True. `5 > 12` is, of course, false. The opposite of that is true!
```

## Conditionals

A core feature of almost all programming languages is conditional blocks. With these blocks, we can execute different pieces of code depending on whether a condition(s) is met.

Consider the following example demonstrating a very simple version of the card game War...

```javascript
var playerOneCard = 6
var playerTwoCard = 10

if(playerOneCard < playerTwoCard) {
  console.log("Player two wins this round!")
}
else if(playerOneCard > playerTwoCard){
  console.log("Player one wins this round!")
}
else{
  console.log("It's a tie!")
}
```

Conditionals will always follow this pattern. There is a key word (`if`, `else if`, `else`), followed by an expression that will evaluate to true or false in parentheses, followed by code to execute when the condition is met.

####Exercise

What are the results of these statements?

```js
//1
!(5 === "5") && (6 > 5) && (1 >= 0)

//2
(5 < 4) || !(3 == 3) && true
```
