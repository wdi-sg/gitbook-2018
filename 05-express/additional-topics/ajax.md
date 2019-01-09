# AJAX
From unit 1:

We defined a place to make a request:
```
var ajaxUrl = "https://swapi.co/api/people/1";
```

Then:
```
// what to do when we recieve the request
var responseHandler = function() {
  console.log("response text", this.responseText);
  console.log("status text", this.statusText);
  console.log("status code", this.status);
};

// make a new request
var request = new XMLHttpRequest();

// listen for the request response
request.addEventListener("load", responseHandler);

// ready the system by calling open, and specifying the url
request.open("GET", ajaxUrl);

// send the request
request.send();
```

Now we know we can change `ajaxUrl` to `http://localhost:3000/something`

Let's implement that:

#### Render a page that makes an AJAX call
```
<script src="/script.js"></script>
```

Create a `script.js` file that includes the above code.

This file needs to go in the public directory in order to be accessible.

Make sure to wrap the ajax code in a `window.onload`

#### Create the route that responds to an ajax call

```
app.get('/something', (request, response) => {
  var stuff = {banana: "monkey"};
  response.send(stuff);
});
```

### Pairing Exercise:
Pick someone's homework app between yourselves to add on to. (it can be any of the SQL express homeworks)

Create an empty javascript file in the public directory.

Create one new route that renders an HTML page with a `script` tag. Reference the javascript file in the script tag.

Make sure this works by running `console.log` in the browser js file.

Create another route that will respond to the ajax calls made by your browser js.

Your browser js should request for a list (array) of data. This can be users, pokemon, songs or any other set of data within your app.

When you recieve the response in the browser, render it to the DOM.

Remember that you'll need this snippet in the express index.js in order to server static files:

```
app.use(express.static('public'))
```

#### further
Implement a form input that the user can type the id of an item to get back the record they were looking for.

#### further
Implement a POST request that creates one of the records
