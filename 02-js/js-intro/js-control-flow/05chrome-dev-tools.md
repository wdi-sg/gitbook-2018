## Chrome Dev Tools Debugging

Using the chrome javascript debug tools we can see what the values of our loops are at any given time.

- open the chrome dev tools on your loops file

Chrome shows you where the currently executing line is
![](http://infoheap.com/wp-content/uploads/2013/07/chrome-developer-tools-js-code-break-point.png)

Use the stepping controls to advance one operation at a time
![](http://infoheap.com/wp-content/uploads/2013/07/chrome-developer-tools-in-debug.png)

Chrome tells you the current value of the variables
![](http://commandlinefanatic.com/art041f001.png)

Chrome tells you about any current errors in your program
![](http://commandlinefanatic.com/art041f008.png)

You can instruct the browser to stop on any line with:
```
debugger;
```

### Exercises
One of the most valuable tools you can use to figure out an error in a loop is to manually inspect values while the loop is running.

This can also simply help you learn what the mechanics of a loop are.

Run the debugger to see the values of each variable at each *iteration* of the loop.

Paste this code into your `script.js`

Answer the questions below.
```
var num = 0;
for (var i = 0; i < 5; i++) {
  console.log("i is " + i);

  num = num + 13;
  num = num * 1.3;
  num = num / 2;
  console.log("num is " + num);

}
```

1.What is the value of `num` when `i` is 3?
1.What is the value of `num` when `i` is 4, and before num is divided by 2?

#### Further:
Run the debugger to investigate the value of all the variables at each iteration of the loop for the examples in the loop page: [06-iterating-over-arrays.md](06-iterating-over-arrays.md)
