# First Express App with Routes

To create a web application using Node, we're going to import a web app server framework called Express. We can install this as a package using npm, then use it to create applications.

## Setting up a project
Create a new folder for use with the project using `mkdir node_calculator`, and cd into `cd node_calculator`

First we want to start a new project by going `yarn init`. Follow the instructions, clicking `enter` through the statements. you many want to specify a version number, but most default options should be fine. It will also specify an initial file to use. The default is `index.js`, and this acts as the "entry point" into our app.

## Basic Express Setup

Before we do anything else, let's set up a basic Express app. We need to install our dependencies, create the index.js server file, and create an index for our homepage.

```bash
yarn add express
touch index.js
```

**Note:** 

We've been running `yarn global add <package name>`to install the package globally. You'll want to reserve `global add` for packages that will be run in the command line.

### index.js

The following example shows how to get routes working in Node. A **route** is a combination of a URL pattern + HTTP Verb (get, post, put, delete). These verbs represent a method for the request.

Each route is called on our Express app, and takes a URL pattern and a callback function. The callback function gives us back the request (`req`) and response to send back to the client (`res`). Calling the `.send` function on the response sends a string back to the client.

```js
const express = require('express')
const app = express()

app.get('/', function(req, res) {
  res.send('hello brian')
});

app.listen(3000)
```

### More Route Styles

By putting a colon before a string in our route, we can create routes with different variables, or **parameters**. These parameters are automatically pulled out for us by Express and can be accessed via the `req.params` object.

```js
const express = require('express')
const app = express()

app.get('/', function(req, res) {
  res.send('hello brian')
});

app.get("/greet/:name/:lastname", function(req, res) {
  res.send("Hello " + req.params.name + " " + req.params.lastname)
});

app.get("/multiply/:x/:y", function(req, res) {
  res.send("The answer is: " + (req.params.x * req.params.y))
});

app.get("/add/:x/:y", function(req, res) {
  res.send("The answer is: " + (parseInt(req.params.x) + parseInt(req.params.y)))
});
```

In addition to having routes where different portions of the URL are different paramaters, we can use the generic string of the URL in our route logic using the wildcard.

```js
app.get("/add/*", function(req, res) {
  let myParams = req.params[0].split("/")
  const result = myParams.reduce(function(total, num) {
    return total + parseInt(num)
  }, 0);
  res.send("The answer is  " + result)
})
```

This will give you a URL like `http://localhost:3000/add/5/3/3/2/3` and give you an answer.

### Running your Project
If `"main": "index.js"` is in your `package.json`, then running `nodemon` will automatically start your project and serving your file.
