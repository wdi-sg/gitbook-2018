#Functional Programming

Programming with functions! Weren't we already doing that? Well yes, but we can use functions more heavily, especially in place of loops.

---

##Objectives

* Use callbacks with setInterval and setTimeout

Previously, we saw that functions can be assigned to variables. For example:

```js
var add = function(a, b) {
  return a + b
}

add(1, 2);
> 3
```

Functions are **first-class citizens** in JavaScript. This means that we can create functions, store them into variables, and pass functions into other functions. Functions are only executed when called. Try this to illustrate:


---

Try running the following. What is printed to the screen?

```js
var bag = function() {
  console.log('Hello, I am a bag');
}

console.log(bag);
```

Inside the bucket of the variable is a function, waiting to be run.

We can take advantage of this behavior by defining **callback functions**. Callback functions are passed via variable name (reference), and are *called* at a specific time.

## Do something later: Callbacks

---

### setTimeout

The `setTimeout()` function takes a function and a delay in
milliseconds, and executes the function as soon as possible after that
delay has passed.

```js
var announce = function() {
  console.log('Ding!');
}

var threeSecondTimeout = setTimeout(announce, 3000);
```

Under the hood the function is being executed just like the one we wrote.

---

This can be done via **anonymous functions** as well. Anonymous functions are functions that are not stored to a variable. They are great for functions you only need to define once. Here's an example.

```js
var fiveSecondTimeout = setTimeout(function() {
  console.log('Ding!');
}, 5000);
```

---

The `setInterval()` function takes a function and a delay in
milliseconds, and executes that function as closely as possible each
time that interval of milliseconds has passed.

```js
var annoy = function(){
  console.log('Are we there yet?');
};

var oneSecondInterval = setInterval(annoy, 1000);
```

---

Things to be careful of: you need to know what the function expects as parameters. Javascript is forgiving, but not a mind-reader.

Oh, and if you want to disable the timers before they fire, you can use the `clearTimeout(timeoutHandle)` or `clearInterval(intervalHandle)` functions:

```js
var fourSecondTimeout = setTimeout(announce, 4000);
clearTimeout(fourSecondTimeout);

clearInterval(threeSecondInterval);
clearInterval(fiveSecondInterval);
```

### Pairing Exercises

1. Write code that uses setTimeout

2. Write code that uses setInterval

3. write a set timeout that sets another timeout inside the callback.

4. write code that uses setInterval and a separate counter variable to console.log something every 3 seconds.

5. write code that clears the interval after 20 seconds
