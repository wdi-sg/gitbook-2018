# Node Modules

### Context
We are now executing our code in a different context, one that no longer relies on `<script>` and `index.html`.

If we want one set of javascript inside file A to interact with other javascript in file B, how do we accomplish that?

The answer is modules.


### Objectives
- Explain the use of modules
- Code a module

## Node Modules

How do we include other javascript into our node.js programs? We are used to `script` and `link`.

If we wanted to have a second or third javascript file accessible to our original file, or CSS that adds or modifies our current CSS, we could just add it:


```
<script src="script2.js"></script>
```

```
<link rel="stylesheet" href="style2.css">
```

**But** our javascript in no longer being executed in the context of a web page.


Luckily node provides a way to use external libraries.

```
// the jquery library is actually all contained in the variable $
const myOtherCode = require('script2.js');
```

Let's see how this works:

### Make your own modules

In essence, if a file puts something inside of module.exports, it can be made available for use in any other file using `require()`.

For example, let's make two files: `touch my-module.js main.js`

`my-module.js`:
```js
// declare a variable in the file
const number = 7;

// add a key and a string to the exports object
module.exports.name = "Kenaniah";


// add an array to the exports object
module.exports.arr = [1, 2, 3];

// add a function to the exports object
module.exports.getNumber = function(){

    // try to access the variable defined above.
    console.log("Get number called. Returning: ", number);
    return number;
}

console.log("End of my-module.js file")
```
---

#### Use the module you created

`main.js`:
```js
// here we're grabbing everything that's "exported" in our other file, and storing it a variable called 'my'
const my = require('./my-module')

// Variables and such that were not exported aren't in scope
console.log("number is " + typeof number) // undefined

// Anything exported can be accessed on the object
console.log("Name is: ", my.name)

// JavaScript is still JavaScript
console.log("The array contains " + my.arr.length + " elements")

// Let's see the module we imported
console.log(my)
```
Then try running:
```
node my-module.js
node main.js
```

### Pairing Exercise
Run the above code.

#### Further
Write a set of calculator functions in a file called `calculator.js`. (add, subtract, multiply, divide)

Export all the functions.

In a new file `index.js` write a command line program that adds 2 + 2 using the functons defined in `calculator.js`.

#### Further
Make the command line program take arguments from the comand line.

Ex:
```bash
node index.js add 2 9
```

#### Further
Write a 3rd file, `circle.js`. This one `require`s `calculator.js` to calculate dimensions of a circle. (area, circumference, etc.)

Add to `index.js` to call the functions you wrote in `circle.js`.

```bash
node index.js circle area 7
```

```bash
node index.js circumference 7
```
