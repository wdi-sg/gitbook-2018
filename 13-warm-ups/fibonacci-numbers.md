# Fibonacci numbers

Write a function that generates and returns the n-th fibonacci number.

From [Wikipedia](https://en.wikipedia.org/wiki/Fibonacci_number):

> In mathematics, the Fibonacci numbers are the numbers in the following integer sequence, called the Fibonacci sequence, and characterized by the fact that every number after the first two is the sum of the two preceding ones.

These are the first 8 fibonacci numbers:

```
0, 1, 1, 2, 3, 5, 8, 13, ...
```

Therefore `fibonacci(8)` should dynamically calculate and return the value 13.

## Further - optionally return a list

Modify your algorithm so that it takes in a second parameter, `returnList`. When the value is true, return an array of the fibonacci numbers up to the n-th number. When false, return the n-th number itself.

The new function signature should be:

```js
function fibonacci(n, returnList) {
    // ...
}
```

ES6 allows default parameter values - would you use one here?

## Further - recursive algorithm

Every algorithm can be implemented iteratively _and_ recursively. One approach may be more suitable than the other in most situations, but it's still feasible to implement both.

In the case of Fibonacci, a recursive algorithm might be easier to reason about and read. Here is a naive recrusive implementation of Fibonacci:

```js
function fibonacci(n) {
    if (n === 0) return 0;
    if (n === 1) return 1;

    return fibonacci(n - 2) + fibonacci(n - 1);
}
```

Yes, you're not seeing things - this function is calling itself. Some might say, it's _recursing_ on itself. This is perfectly valid code.

Play with the above function by adding `console.log` and `debugger` statements and try and understand how it works. Then, delete the code and try implementing it yourself.

## Further - dynamic programming

An algorithm is of no practical use if it runs too slowly. Try computing `fibonacci(100)` with the above naive recursive implementation.

Still waiting? It will take a while. That's because each time the function is called, it is called 2 more times, and then each of those calls the function twice more, and so on. It is doing _a lot_ of computation!

So we would call this particular algorithm (in its current form) a _naive_ algorithm. Why? Because it brute forces its way to the result, re-running the exact computation multiple times. It is extremely inefficient, and takes a long time to complete.

One common way to deal with such problems is trading space for time. Specifically, we can use a concept called dynamic programming to optimise our algorithm. From [Wikipedia](https://en.wikipedia.org/wiki/Dynamic_programming):

> A dynamic programming algorithm will examine the previously solved subproblems and will combine their solutions to give the best solution for the given problem.

Implementing this optimised algorithm is obviously far beyond the limits of this morning exercise. 

After class, try googling for "dynamic programming" and try and implement a recursive algorithm that uses dynamic programming to execute `fibonacci(100)` in under 1 second.

This, as you might imagine, is a commonly asked question by technical interviewers.

Good luck!