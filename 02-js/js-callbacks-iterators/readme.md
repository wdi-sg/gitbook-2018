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
var fiveSecondTimeoutReference = setTimeout(function() {
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

var oneSecondIntervalReference = setInterval(annoy, 1000);
```

---

Things to be careful of: you need to know what the function expects as parameters. Javascript is forgiving, but not a mind-reader.

Oh, and if you want to disable the timers before they fire, you can use the `clearTimeout(timeoutHandle)` or `clearInterval(intervalHandle)` functions:

```js
var fourSecondTimeoutReference = setTimeout(announce, 4000);
clearTimeout(fourSecondTimeoutReference);

var threeSecondIntervalReference = setInterval(annoy, 1000);
clearInterval(threeSecondIntervalReference);
```

### Timeout and Interval Patterns

Count the number of seconds.

```js
var counter = 0;

var count = function(){
  counter++;
  console.log(counter + ' times.');
};

var oneSecondIntervalReference = setInterval(count, 1000);
```

Do one thing and then another.

```
var secondThing = function(){
  console.log('second!');
};

var firstThing = function() {
  console.log('first!');
  console.log('let's do something soon (second thing)');
  setTimeout(secondThing, 2000);
};

setTimeout(firstThing, 2000);
```


### Pairing Exercises

1. Write code that uses setTimeout

2. Write code that uses setInterval

3. Run the above examples.

4. Write 3 set timeouts that console.log something in this order in your code: 1. setTimeout for 4 seconds 2. setTimeout for 2 seconds 3. setTimeout for 2 seconds. What happens?

5. write code that uses setInterval and a separate counter variable to console.log something every 3 seconds.

6. write code that clears the interval after 20 seconds

7. write code that console.logs something once a second 10 times.

8. prompt the user to enter a number. console.log something every second the number of times the user entered

9. if the user enters a number that's larger than 10, reduce the amount of time between console.logs

10. nest the set interval inside a set timeout- if the user doesn't enter anything after 10 seconds, don't run the set interval.
