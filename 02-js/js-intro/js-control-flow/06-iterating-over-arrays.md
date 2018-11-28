# Iterating Over Arrays

*After this lesson, you will be able to:*
- Iterate over an array

## What is iterating over an array?

Iterating over an array is what we call code that looks at every item in an array.
For loops are the most common way to iterate over an array. We can write for loops
in many different ways in order to find out different things about an array.

Sometimes we'll only need to look at each element in an array one at a time.
sometimes we'll need to look at two elements at a time and compare them to each other.

Some problems, like finding the minimum or maximum element in an array, require
us to store extra information in variables outside of an array as we iterate over
it.

When we're iterating over an array we need to be careful to never access an index
that doesn't exist. Arrays always start at index `0` and and at `a.length - 1`.
No negative number is ever a valid index of an array. Empty arrays have a length
of zero and we can't access any index of an empty array.

We'll show you here a collection of the most common ways to iterate over an array.

## Looking at one element at a time

This is the simplest way of iterating over an array. We write a for loop
that starts at `0` and goes while the value of `i` is less than the length
of the array.

Notice that this for loop correctly won't print anything out for an empty array.

```js
var a = [1,4,2,3,6];

for (var i = 0; i < a.length; i++) {
  console.log((a[i]);
}
```


## Other Array Iteration Patterns:


### Counting One Item in an Array

Write a function called `inventory` that accepts an array of strings representing
a store inventory and accepts a string representing a product. Return the total
number of times the product occurs in the array.

```js
  var inventory = ["toothbrush","cap","shampoo","mars bar","banana"];

  var product = "toothbrush";

  var tally = 0;

  for (var i = 0; i < inventory.length; i++) {
    if (inventory[i] === product) {
      tally++;
    }
  }

  console.log( tally );
```

### Array of Objects

Accessing each nested value whether it's an array or an object works similarly.

```js

var cats = [
  {
    name: 'fluffy',
    weight: 12,
    allergies : ['feathers', 'chocolate']
  },
  {
    name: 'susan chan',
    weight: 13,
    allergies : ['tea', 'dust']
  },
  {
    name: 'chee kean',
    weight: 8,
    allergies : ['dairy']
  }
];

for( var i=0; i<cats.length; i++){
  var saying = cats[i].name + ' is ' + cats[i].weight + ' kilos.'
  console.log( saying );

  // use a nested for loop to print out the allergies

  var allergies = cats[i].allergies;

  for( var j=0; j<allergies.length; j++ ){
    console.log( cats[i].name + ' has an allergy to: ' + allergies[j] );
  }

}
```

### Fencepost Problems

Sometimes we need to do special things at the beginning or end of when we're
iterating over an array.

Imagine a fence. A fence looks like this: `|=|=|=|=|'. This fence has five
fence posts and four, uh, pieces of fence.

There's a discrepency there! If we were writing a for loop to build a fence should
it run five times to place each fence post, or should it run four times to place
each piece of fence?

Consider this psuedo code that tries to place four pieces of fence:

```js
for (var i = 0; i < 4; i++) {
  console.log("=");
  console.log("|");
}
```

The output of this code would build a fence like this: `=|=|=|=|`. There are
correctly four pieces of fence, but we're missing a fence post at the beginning!

If we switch the order of calling the `placeFence` and `placePost` methods then
we end up missing a fence post at the end of the fence.

```js
for (var i = 0; i < 4; i++) {
  console.log("|");
  console.log("=");
}
```

This code produces a fence like this: `|=|=|=|=` without the last fence post at
the end.

The proper way to deal with a "fence post" scenario is to place one post before
or after the for loop.


```js
console.log("|");
console.log("=");
for (var i = 0; i < 4; i++) {

  console.log("=");
  console.log("|");
}
```

This code properly produces a fence with posts on each end, and fence pieces
between each post, like this: `|=|=|=|=|`.


### One-Way Gates

Write code that uses an array of integers and outputs the
largest value in the array.

This problem requires us to create varaibles to store extra information outside
the for loop. We'll use if statements inside the for loop to update these variables
when certain conditions are met.

```js
  var a = [3,4,5,6,1,2];
  var largest = 0;

  for (var i = 0; i < a.length; i++) {
    if (a[i] > largest) {
      largest = a[i];
    }
  }

  console.log( largest );
```

Notice that we initialize `largest` outside of the for loop and output it at the
end. We compare each value in the array to the value and rewrite
the value of `largest` if we ever see something in the array larger than it.

There's a problem with initlializing `largest` to zero. Imagine passing an array
of negative numbers to this function. If the largest number in the collection
were `-12` this function would incorrectly return zero!

## String Builders

Some problems require building up a final result. A Classic example is traversing over
a string backwards to produce a reversed string.

Set up a variable outside the for loop to keep track of the final result.

```js
  var s = "hello";

  var result = "";

  for (var i = s.length(); i >= 0; i--) {
    result += s.charAt(i);
  }

  console.log( result );
```

### Reversing an Array
The same idea can be applied to reverse an array. Create a new empty array
and push elements into it.

```
  var a = [7,8,9,10,11];

  var result = [];

  for (var i = a.length - 1; i >= 0; i--) {
    result.push(a[i]);
  }

  console.log( result );
```

### Double For Loops / Nested For Loops

Sometimes it's useful to nest a for loop inside a for loop. This code tests to
see if an array contains unique elements by comparing each item in the array
to every other item.

Use another variable name other than `i` for the second for loop. `i, j, k, n`
are common for loop variable names.

```js

  // look in every index
  for (var i = 0; i < a.length; i++) {

    // compare this index against every other index
    for (var j = 0; j < a.length; j++) {

      // do the comparison
      if (j !== i) {

        if (a[i] === a[j]) {
          console.log("double!: "+a[i]);
        }
      }
    }
  }
```

### Exercise 1:
Create an `index.html` file and `script.js` file.
Run each example one at a time, .
Create a `console.log` for each interation of the array.

Make sure to format the output well so it is clear what is happening.

e.g. `console.log('iteration: '+i)` instead of `console.log(i)`.

### Further:
Investigate different ways go get values out of an array:
- what if you start `i` at different values?
- what if you change the condition to `<=`?
- what if you change the iteration?
- what if you run the loop backwards?


### Further:
Use nested for loops to iterate over this array:
```
var grid = [
  [4,5,6],
  [4,1,3],
  [7,8,2],
];
```

Write code that sums all the numbers and `console.log`s them.

Write code that sums all the numbers in one of the arrays and `console.log`s them. (i.e. `grid[0]`)

Write code that sums all the *even* numbers in one of the arrays and `console.log`s them. (i.e. `grid[0]`)

### Further:
Use nested for loops to iterate over this array:

```
var grid = [
  [
    [4,4,6],
    [4,1,3],
    [2,8,2]
  ],
  [
    [2,3,6],
    [1,1,3],
    [1,5,2]
  ],
  [
    [4,7,7],
    [1,1,3],
    [1,1,2]
  ]
];
```

Write code that sums all the numbers and `console.log`s them.

Write code that sums all the numbers in one of the arrays and `console.log`s them. (i.e. `grid[0]`)

### Further
Investigate the other iteration patterns below. Run them to see how they work.


### Exercise 2: Run some more loops (10 mins)

- Copy the above code examples for other iteration patterns
- Change the values of i
- Change the conditions from *less than* to *less than or equal to* - what happens?
- Change the starting value of i to something else. What happens?
