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
Paste this code into your `script.js`
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

####Questions:
1.What is the value of `num` when `i` is 3?
1.What is the value of `num` when `i` is 4, and before num is divided by 2?
