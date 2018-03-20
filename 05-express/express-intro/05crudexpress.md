# CRUD in Express

We can now get things from the disk and display them as an HTML page to the user.

Next we want to take input from the user that will result in data being saved on our disk.

This is part of CRUD (Create, Read, Update, Destroy).

[Formal definition on wiki](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete)

We will need several pieces to do this:
1. an HTML form
1. a route and response callback that takes that data
1. a place to store that data
1. and, an HTML page to show the user

First, let's talk about how to send data from the browser to the server.

### HTTP Post

#### Demo: How a user sends data over the internet: HTTP POST
Real-life recreation with paper. (One person pretends to be the backend)
- Prepare a request in the browser. verbs we are using: POST
- Send the request to a server
- server recieves the response
- reads the headers
- looks at the contents of the request
- constructs a response
- sends the response
- browser is waiting for the response
- Response has status and status code on the envelope.
- Deal with the server response
- If it's good, display the output to the user
- If it's bad, do nothing, or display an error to the user

Important Points:
- the parameters for the request are in the headers, the data itself is in the body of the request (inside the envelope)
- the point a post request isn't always to save something, but a request where something is saved is almost always a post request

### How to make a POST request in the browser
- one of the main default behaviors of HTML in the browser is that `<a>` tags create `GET` requests and `form` tags create POST requests.

This behavior is always a default behavior given to us by the browser.

Given:
```
<form method="POST" action="/animals">
  Animal Name:
  <input type="text" name="name">
  <input type="submit" value="Submit">
</form>
```

When the submit button is clicked, this form will produce a POST request to the server.

---


#### RESTful Routing

RESTful routing is a scheme to structure your URLS that removes duplication, and looks clean. For each type of resource, you can specify a set of routes easily.

We don't need to rename the route animals, because express will separate things out depending on the HTTP method we use.

| **URL** | **HTTP Verb** |  **Action**|
|------------|-------------|------------|
| /photos/         | GET       | index
| /photos/new      | GET       | new
| /photos          | POST      | create
| /photos/:id      | GET       | show
| /photos/:id/edit | GET       | edit
| /photos/:id      | PATCH/PUT | update
| /photos/:id      | DELETE    | destroy


[Read more on wiki](http://en.wikipedia.org/wiki/Representational_state_transfer)

---

## CRUD in action

To receive this data we need to create a `POST` route in express and the `body-parser` npm module.


### BodyParser

Parsing parameters from a form needs an external module called `body-parser`.

```bash
yarn add body-parser
```

**index.js**
```js
const express = require('express');
const bodyParser = require('body-parser');
const app = express();

// tell your app to use the module
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({
  extended: true
}));
```

Note that we set an attribute `extended` to `true` when telling our app to use the body parser. This attribute determines which library is used to parse data. Discussion on extended [here](http://stackoverflow.com/questions/29175465/body-parser-extended-option-qs-vs-querystring).

Now, if we try to add this backend route, calling `req.body` should contain the form input.

**backend - express route**
```js
app.post('/animals', function(req, res) {
  //debug code (output request body)
  console.log(req.body);
  res.send(req.body);
});
```

### Forms

So far we've learnt to send actions using HTTP Methods, PUT for an update and DELETE for a destroy action. There is a problem however. Browsers currently only support GET and POST, so whenever we send a form it will actually be sent using POST. To overcome this we need to use the [Method Override](https://www.npmjs.com/package/method-override) Package. This will allow us to hide some information in the form that when received and processed in Express will change the method to the correct one. Here's an example

```js
var express = require('express')
var methodOverride = require('method-override')
var app = express()

// override with POST having ?_method=DELETE
app.use(methodOverride('_method'))
```
Example call with query override using HTML <form>:
```html
<form method="POST" action="/resource?_method=DELETE">
  <button type="submit">Delete resource</button>
</form>
```
