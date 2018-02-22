# Block level scoping
This is part of a series of guides written by students from WDI7 Singapore. Originally written by Agnes, Weitong and Dax.

## What is Let and Const?
In ES5 - JS did not have block scoping features, and only had function scoping.
In ES6 - let and const introduced, to allow block scoping features.

- LET: allows you to declare variables that are limited in scope to the block, statement, or expression on which it is used.
- CONST: they are not immutable, but the reference to the variable cannot be changed.

E.g. const cannot be reassigned
```js
var hello = 2;
const bye = 10;
for (var i = 0; i < hello; i++) {
  const bye = 5; // this creates a new const, and hence does not come in conflict with the const bye globally
  console.log(bye);
}
console.log(bye);
```

E.g. let - only exist in the {} scope
```js
for (var i = 1; i <= 5; i++){
  setTimeout(function () {
    console.log(i);
  }, 1000)
}

for (let i = 1; i <= 5; i++){
  setTimeout(function () {
    console.log(i);
  }, 1000)
}
```

For let / const, you can only declare a variable inside of its scope once.
JavaScript
```js
const key = 'abc123';
let points = 50;
let winner = false;
let points = 100;
```
-> The browser says points has already been declared.

Now, what if I had this?

JavaScript
```js
const key = 'abc123';
let points = 50;
let winner = false;

if(points > 40) {
   let winner = true
}
```

The important thing here is that these two winner variables are actually two separate variables. They have the same name, but they are both scoped differently:

let winner = false outside of the if loop is scoped to the window.
let winner = true inside the if loop is scoped to the block.

## Differences between var and let and const
When you declare variables at the bottom with var without assigning any value, it will show up as undefined.
But when you declare variables using const / let without assigning any value, - the const/let variable is uninitialized. This gives reference error, as the const/let variable is initialized only when the let /const/class statement is evaluated
var is function scoped, while const and let are block scoped. const is used when you have a variable that would not be changed, and let is generally used for values that might be changed sometime in the future.

## How do we use Let and Const?
When to use let:
- allows a variable only exist within that block of code.
- if you only use it within that code

When to use const:
- when declaring final variables, apart from the performance reason: if you accidentally try to change or re-declare them in the code, the program will respectively not change the value or throw an error.

## What problem do they solve?
- confusion as to which variables are scoped locally and globally.
In other languages, you are not able to declare the same variables multiple times.

- Garbage Collection - Another reason block-scoping is useful relates to closures and garbage collection to reclaim memory

#Is this the 80% or the 20% i.e. how useful is this feature to us really
20%. We believe that const and let is important enough, that once we master how to use them correctly, we should be able to solve most callback functions that are passing the wrong variables.
