### Route Styles
Even though the rendering of a request is dynamic, slash routes `resource2/resource1` are meant to mimic a file system, in the sense that when you make the request this is the "location" that you are getting the resource from. This is distinct from things like the format (json) that doesn't affect the location of a resource.

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
