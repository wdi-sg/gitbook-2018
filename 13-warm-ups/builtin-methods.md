# Built-in Methods

This is a related series of simulated technical interview questions, with guiding notes.

## Implement each()

Implement a function `each(list, action)` that applies the `action` on every item in `list`:

- `list`: input array
- `action`: function that determines what to do with an item

To test your implementation, pass in a function that console logs an item as the `action` argument, and pass in `[1, 2, 3]` as the `list` argument, as in: `each([1, 2, 3], function () { // ... })`. If your implementation is correct, you should see the numbers 1, 2, and 3 printed to Terminal sequentially.

Hint: This is similar to the built-in [`forEach()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) method in JavaScript.

Note: This is designed to test if you understand how to pass in a function as a parameter of another function - also known as a callback pattern or callback function (referring to the function being passed in, not the one receiving the function).

## Further - implement reduce()

Using your implementation of `each()`, implement a function that reduces an array to a single value by repetitively invoking a `reducer` function on each item in the array, and ultimately returns the final value.

The function signature should be:

```js
reduce(list, reducer, accumulator)
```

- `list`: input array
- `reducer`: function that determines how to combine a current item's value with the existing cumulative value
- `accumulator`: the initial value to start accumulating from

Your `reduce()` implementation should be able to reduce an array of numbers into a single summed up number (one use case of reduce):

```js
const numbers = [1, 2, 3];
const sum = reduce(numbers, function() { }, 0);

console.log(sum);
//=> 6
```

Hints: 

- This is very similar to the built-in [`Array.reduce()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce) in JavaScript - make sure you understand what it's supposed to do before trying to implement it
- What parameters should the `reducer()` callback function have?
- Where does your previously implemented `each()` function fit in this?
- Write pseudo-code to layout your approach generally first before coding

Note: This is designed to test how well you understand callbacks and how to make use of them to solve real problems.

## Resources
 
- Array.prototype.forEach [documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)
- Array.prototype.reduce [documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)
- Possible solution: https://gist.github.com/nickangtc/65b1f5d34ffc79027919391fa1e0c918