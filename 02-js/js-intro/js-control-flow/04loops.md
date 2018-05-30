#Loops

---

## Learning Objectives
- learn the syntax of the three kinds of loops

## Framing
- after conditionals loops are the other piece of control flow in any basic program. Every language you will write in the future will have some kind of loop in it.
- many times in our programs we need to specify a task to happen more than once. Loops help us do that.

![control flow wd40](https://i.imgur.com/v4W1xwD.png)


---

#### `while`

A **while loop** repeatedly executes a code block as long as a specified condition is true.

```js
var i = 0;
while (i < 5) {
  console.log("i is " + i);
  i++;
}

// Will print out:
// >i is 0
// >i is 1
// >i is 2
// >i is 3
// >i is 4
```

---

**The parts of a while loop**

```
while (CONDITION) {
  //CODE
}
```

```
var i=0;

while (i < 5) {

  console.log("i is " + i);

  i++;
}
```

---

Loop that runs forever
```
while(true){
  //CODE
}
```

---

#### `for`

A **for loop** is a fancy **while loop**.

```js
for (var i = 0; i < 5; i++) {
  console.log("i is " + i);
}

// Will _also_ print out:
// >i is 0
// >i is 1
// >i is 2
// >i is 3
// >i is 4
```

---

**The parts of a for loop**

```
for (VARIABLE DECLARATION; CONDITION; UPDATE) {
  //CODE
}
```

---

In other words, you declare a variable and test to see if that variable passes a condition in order to run the code block. The update statement runs after the code block is executed.

Very commonly, you will use it to loop through an array.

```js
var foods = ["pizza", "tacos", "ice cream"];
for (var i = 0; i < foods.length; i++) {
  console.log("i like " + foods[i]);
}

// Will print out:
// >i like pizza
// >i like tacos
// >i like ice cream
```

---

#### `for...in`

A **for...in** loop is similar to a **for loop**, but good for looping
through all the key-value pairs in an Object.

```js
var car = {
  wheels: 4,
  doors: 2,
  seats: 5
};
for (var thing in car) {
  console.log("my car has " + car[thing] + " " + thing);
}

// Will print out:
// > my car has 4 wheels
// > my car has 2 doors
// > my car has 5 seats
```

---

## Backwards For Loops

It's totally possible to run a for loop backwards. We need to do three things:

1. Instead of starting i at zero
simply start it at `var i = a.length - 1`. We have to subtract one from the length
of the array to access the index of the last element to account for zero-based
indexing.
2. Change the test condition to run the for loop while `i >= 0`
3. Change the step instruction to `i--`

```js
for (var i = a.length - 1; i >= 0; i--) {
  console.log(a[i]);
}
```

### Exercise: Run some loops (10 mins)

- Setup Your Files (html / script)
- Copy the above code examples
- Change the value of i to something bigger
- Change the condition from *less than* to *less than or equal to* - what happens?
- Change the starting value of i to something else. What happens?
- Make a while loop that runs 1000 times and console.logs out `i`.
- Make a while loop that runs 1000 times and console.logs out `hello`
- Make a backwards for loop
- Make a loop that increments more than one (counting forwards and backwards) `for(var i=0; i<6; i=i+2)`
- Make a loop that runs a random amount of times (up to a reasonable number) - just google how to create a random integer in javascript if you don't know how.
- Crash your chrome browser tab by running a while loop that console.logs something and goes on forever.
