# Data Types and Structures

---

## Learning Objectives

- List and describe the primitive data types and common complex data types
- Practice proper JS syntax and semantic variable naming
- Describe uses of mathematical operators in Javascript
- Define type coercion

## Framing
- data is the first thing we learn in any programming language. This is how javascript does it.


Primitive data types are the building blocks of Javascript. Whenever you do anything in Javascript, you are creating and changing these basic pieces of information.


---


### Comments
You write comments in your code for yourself and for anyone reading your code in the future.

Comments come in two forms

### Line comments

```js
// descriptive stuff
```
### Multi-line comments

```js
/**
  These
  are
  comments on
  many lines
*/

```

---

## Primitive Data Types

There are five primitive data types that are used most commonly used in Javascript.

  1. Numbers
  2. Strings
  3. Booleans
  4. Undefined
  5. Null

---

## Variables

We store data types in variables. A variable is a "bucket" that holds data. You can pass the bucket around, empty it, refill it, etc.

```js
var myClass = "WDI16"
// var - indicates a variable is being defined
// myClass - the name of the variables
// "WDI16" - the value being assigned to the variable
```

After declaration you can then reference variables by just their name, without "var"...

```js
myClass
// => "WDI16"
```

---

## Operations

Math in Javascript follows the same rules you've known since elementary school math.
- Basic operators: `+, -, *, /`

```js
// Addition
10 + 2
// => 12

// Subtraction
10 - 2
// => 8

// Multiplication
10 * 2
// => 20

// Division
10 / 2
// => 5
```

---

Like normal math, Javascript follows the traditional order of operations: P.E.M.D.A.S. or "Please Excuse My Dear Aunt Sally." Mathematical operations are executed in the following order...
  1. Parenthetical expressions
  2. Exponentiation
  3. Multiplication
  4. Division
  5. Addition
  6. Subtraction

```js
(4 + 2) * (12 / 3)
// => 6 * 4
// => 24

(8 / 4 * 2) + 1
// => (2 * 2) + 1
// => 5
```

---

### % (Modulus)

The modulus operator - `%` - returns the remainder of a division operation.

```js
12 % 5
// => 2, which is the remainder of 12 / 5
```

Modulus has a pretty handy use case: to check if a number is even. We can do this by running `NUMBER % 2`. If a number is even, the result should be 0 (i.e., there should be no remainder).

```js
4 % 2
// => 0, because 4 is even

5 % 2
// => , because 5 is odd. When 5 is divided by 2, the remainder is 1.
```


---

### NaN ("Not a number")

A special number...that's not a number?

```js
typeof NaN
// => Number
```

You usually get `NaN` when the result of a math operation is not real (e.g., dividing 0 by 0, multiplying strings together).

```js
0/0
// => NaN
```

---

## Undefined & Null

`undefined` and `null` are values that indicate the lack of a meaningful value.
- Anybody else find that weird? How is there more than one data type for nothing?
- Q: What's the difference?

---

### Undefined

`undefined` is automatically applied to any variable with no value.

```js
typeof undefined
// => undefined
// => It evaluates as itself because it is a primitive data type

var nothing
// => undefined
// Any property that has not been assigned a value is undefined

```


---

### Null

`null` is an explicitly-assigned non-value.
(undefined is an implicity assigned non-value)
- Javascript will never set anything to `null` by itself. `null` only appears when you tell it to.
- It is useful as a placeholder for a variable that you know will be replaced with an actual value later on
---

> So the main difference between `undefined` and `null` is intention. Other than that, they're both...nothing.

---

## Type Coercion

Javascript will try to make sense of any strange operations you throw at it.
- Examples of "strange": subtracting a number from a string, multiplying `null` by 100
- It does this by converting data types using a process called "type coercion"

You might encounter this when dealing with numerical values but for whatever reason some of them are in string form.

```js
// In some cases Javascript is helpful and converts strings to numbers in the correct way.
"3" - "2"
// => 1

// ...but sometimes it doesn't. In this example, the + operator acts as if it's concatenating two strings.
"3" + "2"
// => 32

// And this?
"five" * 5
// => NaN
```

---

When in doubt, convert data types that should be numbers using `parseInt()`.

```js
parseInt("3")
// => 3
// parseInt converts a string to a number value, if available.

parseInt("burrito")
// => NaN, because "burrito" cannot be converted into a number
```

There are other examples of type coercion, but the point here isn't to remember them all. Just be aware that sometimes Javascript will fire weird results back at you with no explanation. Sometimes, type coercion might be the culprit.


---

## Strings

Strings are words in javascript! We instantiate strings using the "string literal" form.

```js
// Can use single quotes to instantiate a string...
var greeting = 'Hello!'

/// ...or double quotes.
var greeting = "Hi there!"
```

---

### Concatenation

Like numbers, you can add strings together using `+`...
**`+` looks the same in the context of 2 strings, but does a different job**

```js
var city = "Washington, "
var state = "DC"
var address = city + state
// => "Washington, DC"
```

You can't, however, use other math operators on strings...

```js
"hamburger" - "ham"
// => NaN

"hamburger" * 3
// => NaN
```

---

## Data Structures

---

### Arrays

Arrays are ordered collection of related data types and are organized by index.
- Indexing begins at 0 (e.g., first element in an array has an index of 0, the second has an index of 1, and so on).

---

We instantiate an array like this...

```js
// Instantiate with an array literal.
var mountRushmore = [ "Washington", "Jefferson", "Roosevelt" ]
```

And access its values like so...

```js
mountRushmore[0]
// => "Washington"

mountRushmore[1]
// => "Jefferson"

mountRushmore[2]
// => "Roosevelt"

mountRushmore.push("Lincoln")
// mountRushmore = [ "Washington", "Jefferson", "Roosevelt", "Lincoln" ]

mountRushmore[3]
// => "Lincoln"

// You can also place arrays within arrays.
var letters = [ ["a","b","c"], ["d","e","f"], ["g","h","i"] ]

// How would we go about accessing the letter "f" in the above array?
letters[1][2]
// => "f"
```

---

### Array Methods

There are a lot of useful methods that come with Javascript we can use to inspect and modify arrays. To learn what some of them are...
* `.length`
* `.push`
* `.indexOf`

> There are many more, but these are the most widely-used.

---


### Objects
Objects use "keys" instead of indexes to store data.

Why use objects to store `key` and `value` pairs? They are like arrays except that data is not stored in any sorted order and keys do not have to numbered indexes.

---

#### Creating

```js
var friend = {firstName: "Jane", lastName: "Doe"}
```


---

#### Accessing

```js
friend.firstName
friend.lastName

friend['firstName']
friend['lastName']
```

---

### Documentation

Navigating documentation is a great skill to have. Some sets of documentation are harder to navigate than others, but if you have a sense of how to dig through a massive trove of information like MDN or RubyDocs, you'll become a much more efficient programmer.

> [MDN Array Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)

> [https://github.com/wdi-sg/js-primitives](https://github.com/wdi-sg/js-primitives)
