#Functions

---

##Objectives
* Define a function
* Define a function with a parameter
* Define a function that operates on two parameters
* Create functions with and without return values

---

##Defining a function

A function is a module that can store and invoke code. When writing repetitive
code, we can isolate code into **functions** in order to reduce repetition. For
example, if we needed to say "Hello World" to the screen multiple times, we can
create a function like so.

Function is synomymous with method.

We've been using functions already without knowing it:
```
alert('hello');
```

Those are predefined.
But we can define our own:

```js
var greeting = function() {
  console.log("Hello World");
}

greeting();
```

---

We can **call** the function by taking the variable name and appending parentheses to the end of the function variable.

**Parts of a function**

```
function FUNCTIONNAME() {
	//CODE
}
```

---

We can also create functions that accept **parameters**, and use those parameters as variables in the function.

##Defining a function with a parameter
```js
function greeting(taco) {
	// anything inside of here will execute when called
	console.log("Good morning", taco);
}

var name = "Josh"
var name2 = "Brian"
greeting(name);
greeting(name2);
```

---

##Defining a function with two parameters

Functions can have multiple parameters, separated by commas.

```js
function greeting(person, stuff) {
	// anything inside of here will execute when called
	console.log("Good morning", person, stuff);
	console.log("person:", person);
	console.log("stuff:", stuff);
}

var name = "Josh";
var thing = "Spoon";
greeting(name, thing); // "Good morning Josh Spoon"
greeting(thing, name); // "Good morning Spoon Josh"
```

---

##Return Values

Note that functions can have **input** via parameters. They can also have **output** as return values. Returning values from a function is denoted by the keyword `return`. **Something** is always returned by the function. If you don't specify anything, it returns `undefined`.




```js

var add = function(a,b){
  return a + b;
};

add(2,3);

var number = add(2,4);

console.log( number + 3 );

// what happens when we call this?

var add = function(a,b){
  a + b;
};

```

---

With console.log

```
// With a return value
var returnHello = function(name) {
	return("Hello, " + name);
}

console.log("with a return value:");
console.log(returnHello("jane") );

// Without a return value
var returnHello2 = function(name) {
	console.log("inside returnHello2: Hello, " + name);
}
returnHello2("nachos");
console.log("without a return value:");
console.log( returnHello2("taco") ); //will show as undefined
```


---





Note that printing something to the screen using `console.log` is not the same as returning values.

```js
var multiply = function(num1, num2) {
	console.log("inside the function");
	// return result = num1 * num2;
	return num1 * num2;
}

var firstNum = 2;
var secNum = 3;
var taco = multiply(firstNum,secNum);

console.log(firstNum + " multiplied by " + secNum + " is " + taco );
```

---

##Declaring functions

There are two different ways to declare a function
```js
function multiply(a, b) {
	return a * b;
}

var multiply = function(a, b) {
	return a * b;
}
```

The difference between these two is that the first one is defined at run-time, meaning that if we try to call the function before it's declared, an error will be thrown:
```js
multiply(2, 2); // ERROR

var multiply = function(a, b) {
    return a * b;
}
```

The second declaration is defined at parse-time, so we can call the function wherever we'd like.
```js
multiply(2, 2); // success

function multiply(a, b) {
	return a * b;
}
```

Despite being more flexible, the former declaration that assigns the function to a variable is more common when developing Node applications.


---


###Exercises

- What is the return value of this function when called?

```js
var lightsabers = function(num) {
	console.log('I have ' + num + ' lightsabers.');
}

lightsabers(2);
```

- How would the function above be modified if the user wanted to pass in an object of lightsabers, like this one?

```js
var myLightsaberCollection = {
	blue: 1,
	green: 3
}

var lightsabers = function(lightsaberCollection) {
	//code here
}

lightsabers(myLightsaberCollection);

// Output
// I have 1 blue lightsaber
// I have 3 green lightsabers
```

- paste this code into your script.js file

```
var doMaths = function(a,b){
  var result = 0

  result = a + b;
  result = result + b + b + 3;

}

for( var i=0; i<5; i++ ){
  doMaths(i, 2);
}
```

Question:
Using the chrome debugger, what is the value of the result inside the function when `i` is equal to 3?
