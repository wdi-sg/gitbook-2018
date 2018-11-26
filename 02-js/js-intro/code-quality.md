# Intro to code quality

### Javascript Style
[Style Guide](../01-workflow/js-styleguide.md)

aside: Check out the official class javascript styleguide (from AirBnb): This covers all of the javascript syntax- even things we won't be covering in class. [https://wdi-sg.github.io/gitbook-2018/01-workflow/js-styleguide.html](https://wdi-sg.github.io/gitbook-2018/01-workflow/js-styleguide.html)
Or: Here's a list of optional reading materials for [JavaScript](http://javascript.crockford.com/code.html) from Douglas Crockford, an OG Yahoo programmer.


We want to make sure that your code not only works, but is understandable to other people (think about it: you won't be the only person reading this AND it might be weeks or months between you looking at your code - make it understandable).

### Indentation

Indentation denotes hierarchy in your code. When writing code in JavaScript, you should indent code that is being run inside other code. Here's an example:

**Correct:**
```
if (5 % 2 ==0) {
    console.log('Number is even');
} else {
    console.log('Number is odd');
}
```
Note that the console.log that will be run inside the *if* portion is tabbed over once to denote that it should be run if this portion of the code is executed.

**Incorrect:**
```
if (5 % 2 ==0) {
console.log('Number is even');
} else {
console.log('Number is odd');
}
```

Note that this code will still run, but it is hard to read! This will make your coworkers (and instructors) a little sad :confused: because it will cause more brain work for us. You want to make it as easy as possible for others to know what you are doing, so please show the relationships with indentation!

### Semantic naming
when naming your variable, be explicit! Skip naming your variables `x` or `y` when you can name them `timeTracker` or `uncleRoysChickenCount`

:elephant: Remember, JavaScript naming convention is *camel case*. This means, the first word in the name is lowercase and any additional words in the name are connected (no spaces between words) and the first letter is capitalized `camelCase` or `thisIsCamelCase`.

### Case Sensitivity
Be aware that names are case sensitive. That means: `var redcow` is **not** the same as `var redCow`.

### Comments
Comments are notes to others and your future self. The more difficult a section of code is for you to write, the more important it is that you write a comment for that line, explaining what it does.

```js
// if there is no remainder of the number, it's even.
if (number % 2 ==0) {
console.log('Number is even');
} else {
console.log('Number is odd');
}
```

### refactoring
If your comment is very long, or it takes a while for you to explain what a piece of code does, consider that it might be good to refactor the code in some way- but only after you have it working and committed into git.
