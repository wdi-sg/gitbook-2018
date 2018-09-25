### Route Styles
Even though the rendering of a request is dynamic, slash routes `resource2/resource1` are meant to mimic a file system, in the sense that when you make the request this is the "location" that you are getting the resource from. This is distinct from things like the format (json) that doesn't affect the location of a resource.

By putting a colon before a string in our route, we can create routes with different variables, or **parameters**. These parameters are automatically pulled out for us by Express and can be accessed via the `request.params` object.

```js
const express = require('express')
const app = express()

app.get('/', (request, response) => {
  response.send('hello brian')
});

app.get("/greet/:name/:lastname", (request, response) => {
  response.send("Hello " + request.params.name + " " + request.params.lastname)
});

app.get("/multiply/:x/:y", (request, response) => {
  response.send("The answer is: " + (request.params.x * request.params.y))
});

app.get("/add/:x/:y", (request, response) => {
  response.send("The answer is: " + (parseInt(request.params.x) + parseInt(request.params.y)))
});

// make a search using: localhost:3000?q=pokemon
app.get("/search", (request, response) => {
  response.send("You are searching for: " + request.query.q);
});
```

In addition to having routes where different portions of the URL are different paramaters, we can use the generic string of the URL in our route logic using the wildcard.

```js
app.get("/add/*", function(req, response) {
  let myParams = req.params[0].split("/")
  const result = myParams.reduce(function(total, num) {
    return total + parseInt(num)
  }, 0);
  response.send("The answer is  " + result)
})
```

This will give you a URL like `http://localhost:3000/add/5/3/3/2/3` and give you an answer.

### Query Parameters
Query parameters are used to tell the server that you have a query for the resource you are trying to access. Google search bar URLs are formatted this way: `google.com/search?q=bananas`

*Access Query Parameters:*

`http://localhost:3000?hello=bye` query parameter can be accessed with `request.query.hello`.

Notice that these are keys and values. `hello` becomes a key in the `query` object.

### Pairing Exercise:
- Add each of the routes above into your app. Copy each one separately!
- `console.log` the value of request
- Try changing the `:name` of the url parameter to see the key change
- Try putting in a query string in addition to the URL parameters
