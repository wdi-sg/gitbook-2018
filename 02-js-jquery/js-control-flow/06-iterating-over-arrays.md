# Iterating Over Arrays

*After this lesson, you will be able to:*
- Iterate over an array
- find minimum, maximum values in an array
- handle fencepost problems
- tally the most frequently occuring item in an array
- prevent errors from accessing non-existent indexes

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
for (var i = 0; i < a.length; i++) {
  console.log((a[i]);
}
```

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

## Fencepost Problems

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
  placeFence();
  placePost();
}
```

The output of this code would build a fence like this: `=|=|=|=|`. There are
correctly four pieces of fence, but we're missing a fence post at the beginning!

If we switch the order of calling the `placeFence` and `placePost` methods then
we end up missing a fence post at the end of the fence.

```js
for (var i = 0; i < 4; i++) {
  placePost();
  placeFence();
}
```

This code produces a fence like this: `|=|=|=|=` without the last fence post at
the end.

The proper way to deal with a "fence post" scenario is to place one post before
or after the for loop.


```js
placePost();
for (var i = 0; i < 4; i++) {
  placeFence();
  placePost();
}
```

This code properly produces a fence with posts on each end, and fence pieces
between each post, like this: `|=|=|=|=|`.

### A Classic Fence Post Problem

Write a function called `printArray` that accepts an array of integers and prints
out each of the integers with a comma between each item. If the array is empty,
the function should print nothing. If there is only one item
in the array the function should just print the one item without any commas, like
this `42`. An array with more items should be printed like this: `42,12,97,8'.

Try to code this on your own before looking at the solution.

```js
function printArray(a) {
  // Deal with the fence post by printing the first item in the array
  // without a comma and only when we're it is a non-empty array!
  if (a.length > 0) {
    console.log(a[0]);
  }

  // Deal with each piece of the fence by printing a comma, then the
  // value of the array at each index. Start the for loop at `i = 1`
  // to account for the fact that the first item was already printed.
  for (var i = 1; i < a.length; i++) {
    console.log("," + a[i]);
  }

  // Include an empty println statement at the end to make the output
  // produce a newline character at the end.
  console.log();
}
```

You may find it more natural to deal with the fence post after the for loop.
You can do this too.

```js
function printArray(a) {
  // Don't print anything when the array is empty. Simply return to exit
  // the function.
  if (a.length === 0) {
    return;
  }

  // Start i at zero and print each element in the array followed by a comma.
  // Set the for loop to end before `a.length - 1` to leave one element left
  // for the fence post at the end after the for loop.
  for (var i = 0; i < a.length - 1; i++) {
    console.log(a[i] + ",");
  }

  // Print the final element at the end of the list without a comma.
  console.log(a[a.length - 1]);
}
```

## One-Way Gates

Write a function called `max` that accepts an array of integers and returns the
largest value in the array.

This problem requires us to create varaibles to store extra information outside
the for loop. We'll use if statements inside the for loop to update these variables
when certain conditions are met.

```js
function max(a) {
  var largest = 0;

  for (var i = 0; i < a.length; i++) {
    if (a[i] > largest) {
      largest = a[i];
    }
  }

  return largest;
}
```

Notice that we initialize `largest` outside of the for loop and return it at the
end of the function. We compare each value in the array to the value and rewrite
the value of `largest` if we ever see something in the array larger than it.

There's a problem with initlializing `largest` to zero. Imagine passing an array
of negative numbers to this function. If the largest number in the collection
were `-12` this function would incorrectly return zero!

```js
// make sure the array isn't empty
if (a.length > 0) {
  // set the 
  var largest = a[0];
}
```


Think of this as a one-way valve that only ever goes up.

## String Builders

Some problems require building up a final result. A Classic example is traversing over
a string backwards to produce a reversed string.

Set up a variable outside the for loop to keep track of the final result.

```js
function reverseString(s) {
  var result = "";

  for (var i = s.length(); i >= 0; i--) {
    result += s.charAt(i);
  }

  return result;
}

reverseString("burrito"); // returns "otirrub"
```

## Reversing an Array
The same idea can be applied to reverse an array. Create a new empty array
and push elements into it.

```
function reverseArray(a) {
  var result = [];

  for (var i = a.length - 1; i >= 0; i--) {
    result.push(a[i]);
  }
  
  return result;
}

reverseArray([1,2,3,4,5]); // returns [5, 4, 3, 2, 1]
```

## Counting One Item in an Array

Write a function called `inventory` that accepts an array of strings representing
a store inventory and accepts a string representing a product. Return the total
number of times the product occurs in the array.

```js
function inventory(inventory, product) {
  var tally = 0;

  for (var i = 0; i < inventory.length; i++) {
    if (inventory[i] === product) {
      tally++;
    }
  }

  return tally;
}
```

## Double For Loops / Nested For Loops

Sometimes it's useful to nest a for loop inside a for loop. This code tests to
see if an array contains unique elements by comparing each item in the array
to every other item. 

Use another variable name other than `i` for the second for loop. `i, j, k, n`
are common for loop variable names.

```js
function isUnique(a) {
  for (var i = 0; i < a.length; i++) {
    for (var j = 0; j < a.length; j++) {
      if (j !== i) {
        if (a[i] === a[j]) {
          return false;
        }
      }
    }
  }
  return true;
}

isUnique([1,2,3,3,4]) // returns false
isUnique([1,2,3,4])   // returns true
isUnique([])          // returns true
```

## Mousetrap
A mousetrap (these are made up names) is something that snaps closed once
an never opens again. 

```js
function isUnique(a)  {
  boolean isUnique = true;

  for (var i = 0; i < a.length; i++) {
    for (var j = 0; j < a.length; j++) {
      if (j !== i) {
        if (a[i] === a[j]) {
          isUnique = false;
        }
      }
    }
  }

  return isUnique;
}
```
