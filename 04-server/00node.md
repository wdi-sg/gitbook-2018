# Intro to Node.js

---

### Objectives
- Explain what Node.js is & why it exists
- Compare and contrast Node/Express vs. Ruby/Rails

---

## What is Node.js?

![](http://jce-il.github.io/ASOSMA/firefox-ios/general.jpg)

---
![](https://i.imgur.com/Qgz5eFD.png)

<span class="non-slide"></span>

The makers of Node.js took javascript (which normally only runs in the browser) and made it available in your computer (on the server side). They took Google's V8 JavaScript Engine and gave it the ability to compile JS programs into machine code.

Keep in mind, Node.js is strictly a tool to run JavaScript on a server – while it's possible to build web applications and APIs in straight JS, we'll actually be using a framework on top of Node called Express.

---

#### Asynchronous

On top of that, one of the big differences is that Node.js is designed to be _event-driven_ and _asynchronous_. While earlier frameworks can only do one thing at a time, Node purposefully sends nearly everything to the background and keeps going.

---

Just like a click event is something that is designated to happen at another time, we will see that server side processing of a request follows a similar pattern- one where we actaully have no idea when or any control over when we can successfully send a response to the request.

---

Relative "distances" are actually quite far when in the context of executing a program. (In other words, this metaphor is to scale in the mathematical sense)
![https://blog.codinghorror.com/content/images/2014/May/storage-latency-how-far-away-is-the-data.png](https://blog.codinghorror.com/content/images/2014/May/storage-latency-how-far-away-is-the-data.png)

[https://blog.codinghorror.com/the-infinite-space-between-words/](https://blog.codinghorror.com/the-infinite-space-between-words/)

---

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
- Designed to make real time applications

---

#### Installing Node.js

To check if we already have Node installed, type: `node -v` in terminal. You will see the Node version if it's installed.

If it's not installed, you can install from the Node.js website, or better yet, use Homebrew like this:

```
brew install node
```

One of the advantages of using Homebrew is that you can update your versions easily like this:

```
brew upgrade node
```

This will install both Node.js and npm

---

#### Interactive Node

If you simply type node in terminal, you will launch Node's REPL (Read-Eval-Print-Loop) interactive utility. It works similar to the chrome dev tools console. Let's test it:

---
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

> process
// [an object with a long list of properties that comes together with node]
```

Press control-c twice to exit REPL.

---

#### Executing a JS program

Write and execute some code in a file! In your working directory:

`mkdir first-node`
`cd first-node`
`touch main.js`
`sublime main.js`

Paste into the file:
```
console.log("hello");
```

`node main.js`

---

## [Process](https://nodejs.org/api/process.html#process_process)
The `process` object is a `global` that provides information about, and control over, the current Node.js process. As a global, it is always available to Node.js applications without using `require()`. Two most commonly used `process` property are `process.argv` and `process.env`.

"Process" refers to one CPU execution environment. Similar to how one chrome tab exists within the browser.

---

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

---

### Unix environment variables: A Refresher
When we write apps, we have certain global "environment" variables that are different for each context or **installed environment**. -The enviroment that you are running the app in.

Examples:

- a variable for whether or not the app is running on your mac computer, or on a server, or on a "cloud" virtual server.
- a variable that holds the API credentials for accessing a Google API - (one for testing and one for "production")
- a variable that hold the location of a certain configuration file or any disk location: `/Application/Google\ Chrome` etc.

---

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
---

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

---

#### Things to Note

A `module` isn't actually a global object, but rather, it is local to each module (i.e. the file it is being defined in). However, we can use the `exports` property on modules (`module.exports`) to specifically declare what from the module we want to be made available to other modules/files through the use of `require()`

> Note: The module's source file is only executed the first time that file is required.

---

### Pairing Exercises:

1. Use the `node` command to open the REPL and do some basic math.

1. Write a basic node program that console.logs something
```
touch index.js
```
Write something like:
```
console.log("hello");
```
Run it:
```
node index.js
```

1. `console.log` the value of `process.argv` in your program

1. Write a basic node program that takes user input and `console.log`s it back out to the user:

So writing this on the command line:
```
node index.js yourArgumentHere
```

Should result in:
```
yourArgumentHere
```

In your `index.js` file, in order to get your first argument, you will need:
```
process.argv[2]
```

Extra Question:
What is the value of `process.argv[0]` and `process.argv[1]`?

1. Add the ability to put a second argument for your command line program. `console.log` that as well.

1. Add the ability to add an unlimited amount of extra arguments to your program. `console.log` each one

1. Create a nodejs command line program that takes 2 arguments and adds them together.

1. Create a nodejs command line program that takes arguments and adds them all together.
