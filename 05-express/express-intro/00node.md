# Intro to Node.js

### Objectives
- Explain what Node.js is & why it exists
- Compare and contrast Node/Express vs. Ruby/Rails
- Use module.exports and require to organize code

## What is Node.js?

The makers of Node.js took javascript (which normally only runs in the browser) and made it available in your computer (on the server side). They took Google's V8 JavaScript Engine and gave it the ability to compile JS programs into machine code.

Keep in mind, Node.js is strictly a tool to run JavaScript on a server – while it's possible to build web applications and APIs in straight JS, we'll actually be using a framework on top of Node called Express.

#### Why are people excited about Node?

It's new and hot in the industry but why does it matter?

A lot of developers and companies are excited because it allows you to build fast, scalable APIs and sites in JavaScript. We're _familiar_ with JS and being able to use it on the backend gives us the option to use a single programming language throughout an entire full-stack application.

#### Asynchronous

On top of that, one of the big differences is that Node.js is designed to be _event-driven_ and _asynchronous_. While earlier frameworks can only do one thing at a time, Node purposefully sends nearly everything to the background and keeps going.

Imagine a paper delivery boy riding on his bike delivering papers every morning. Imagine he stops at each house, throws the paper on your doorstep, and waits to make sure you come out & pick it up before moving on to the next house. That would be what we'd call _blocking_ – each line of code finishes before moving on to the next line of code.

Now imagine the paperboy throwing the newspaper on your porch but never stopping his bicycle; never stopping, he just keeps throwing papers on porches, so that by the time you pick it up he'll be 3 or 4 houses down. That would be _non-blocking input/output (I/O)_, or _asynchronous_. This is an extremely awesome ability of node since I/O tends to be very "expensive."

More details on JS callback queue and call stack here.

{% youtube %}
https://www.youtube.com/watch?v=8aGhZQkoFbQ
{% endyoutube %}

#### Ruby/Rails vs. JS/Node/Express

While not strictly a competition (one of the skills you have to practice is knowing what frameworks you should use in which situations), let's compare the technologies:

__Why Choose Ruby/Rails?__

- Quickest path to building app with full CRUD
- Better at working with complex data relationships - ActiveRecord rocks!
- When full page refreshes aren't an issue
- Synchronous programming is simpler in building a straightforward program because you don't have to think about writing code to accommodate for concurrency

__Why Choose Node/Express?__

- JavaScript is everywhere; one language to rule them all
- Asynchronous means generally faster performance
- Better _concurrency_ – it can serve data to more users with fewer computer resources
- Designed to make real time applications

#### Installing Node.js

To check if we already have Node installed, type: ``node -v`` in terminal. You will see the Node version if it's installed.

If it's not installed, you can install from the Node.js website, or better yet, use Homebrew like this:

```bash
brew install node
```

One of the advantages of using Homebrew is that you can update your versions easily like this:

```bash
brew upgrade node
```

This will install both Node.js and npm.

## Getting reacquainted with JS

Before we go further, you should try test it out.  There are two ways to do this – try them both.

#### Interactive Node

If you simply type node in terminal, you will launch Node's REPL (Read-Eval-Print-Loop) interactive utility. Think of REPL as Node's version of Ruby's IRB. Let's test it:

```js
node

> 10 + 5
// 15

> const a = [ 1, 2, 3];
// undefined

> a.forEach(function(v) {
... console.log(v);
... });
// 1
// 2
// 3

> const http = require('http');
// undefined

> http
// [ a massive 'http' object returned from the 'http' module ]

> process
// [an object with a long list of properties that comes together with node]
```

Press control-c twice to exit REPL.

#### Executing a JS program

Write and execute some code in a file! In your working directory:

```bash
mkdir first-node
cd first-node
touch main.js
echo "console.log('hello world!');" >> main.js
node main.js
# hello world!
```

## Node Modules

Like most other modern languages, Node is modular. In essence, if a file puts something inside of module.exports, it can be made available for use in any other file using `require()`.

For example, let's make two files: `touch my-module.js main.js`

```js
// my-module.js
const number = 7
module.exports.name = "Kenaniah"
module.exports.arr = [1, 2, 3]
module.exports.getNumber = function(){
    console.log("Get number called. Returning: ", number)
    return number
}

console.log("End of my-module.js file")
```


```js
// main.js

// here we're grabbing everything that's "exported" in our other file, and storing it a variable called 'my'
const my = require('./my-module')

// Variables and such that were not exported aren't in scope
console.log("number is " + typeof number) // undefined

// Anything exported can be accessed on the object
console.log("Name is: ", my.name)

// Closures are still closures
console.log("The number is: " + my.getNumber())

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

## [Process](https://nodejs.org/api/process.html#process_process)
The `process` object is a `global` that provides information about, and control over, the current Node.js process. As a global, it is always available to Node.js applications without using `require()`. Two most commonly used `process` property are `process.argv` and `process.env`.

### process.argv
The `process.argv` property returns an array containing the command line arguments passed when the Node.js process was launched. The first element will be [process.execPath](https://nodejs.org/api/process.html#process_process_execpath).

The second element will be the path to the JavaScript file being executed. The remaining elements will be any additional command line arguments.

For example, assuming we're launching this node project:
```
$ node process-args.js one two=three four
```  
the output will be:
```
0: /usr/local/bin/node
1: /Users/mjr/work/node/process-args.js
2: one
3: two=three
4: four
```

### process.env
The process.env property returns an object containing the user environment. Typically, the object looks like this.
```
{
  TERM: 'xterm-256color',
  SHELL: '/usr/local/bin/bash',
  USER: 'maciej',
  PATH: '~/.bin/:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin',
  PWD: '/Users/maciej',
  EDITOR: 'vim',
  SHLVL: '1',
  HOME: '/Users/maciej',
  LOGNAME: 'maciej',
  _: '/usr/local/bin/node'
}
```
Practically, updating the `env` property will allow programs to run different coding flow depending on different development environment.

For example
```
// given the process.env.NODE_ENV === 'development'

// setting two different database url for different environment

if(process.env.NODE_ENV === 'development') {
  mongoose.connect(<local db uri>)
} else {
  mongoose.connect(<production db uri>)
}
```

#### Things to Note

A `module` isn't actually a global object, but rather, it is local to each module (i.e. the file it is being defined in). However, we can use the `exports` property on modules (`module.exports`) to specifically declare what from the module we want to be made available to other modules/files through the use of `require()`

> Note: The module's source file is only executed the first time that file is required.

## Review
- What are some of the important distinguishing features of Node?
- Demonstrate how to run JS on your computer, both interactively and in a file
- Demonstrate how `module.exports` & `require` work
