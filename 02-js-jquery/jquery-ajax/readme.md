#Intro to AJAX

##Objectives

* Define AJAX
* Describe the purpose of AJAX
* Utilize AJAX to fetch data from third-party APIs
* Understand the place of AJAX in the request-response cycle
* Describe how promises work in the context of AJAX requests

Ajax (Asynchronous JavaScript and XML) is used to create asynchronous web applications. This simply means a web page that can make calls back to the server in the background.

##Understanding HTTP

Before we can understand AJAX we need to a little background on HTTP. HTTP is one of many internet protocols and is the protocol used for web communcation. HTTP can only send TEXT and is a request-response protocol. This means that a server can only respond to a request from a client (usually a web browser). [Read more on wiki](http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol).


![http diagram](./http_diagram.png)

##AJAX

Originally the only way an HTTP request could be initiated was if the user clicked a link on a webpage, or submitted a form. AJAX allows JavaScript to send requests to the server with or without user interaction. This enables page content to be updated dynamically without a full page refresh.

In Vanilla JavaScript to make a request, we use the XMLHttpRequest class. For example, if we wanted to query the Reddit API for cats, we can do:

```js
  var request = new XMLHttpRequest();
  request.open('GET', 'https://www.reddit.com/search.json?q=kittens', true);

  request.onload = function() {
    if (request.status >= 200 && request.status < 400) {
      // Success!
      var resp = request.responseText;
      console.log(resp);
    } else {
      // We reached our target server, but it returned an error
      console.log('Uh oh, an error on the server side');
    }
  };

  request.onerror = function() {
    // There was a connection error of some sort
    console.log('Something went wrong with the client side.');
  };

  request.send();
```

Let's walk through this for a second. AJAX uses HTTP to request XML (or JSON), so we're going to open a new request and tell it which verb we need and the URL.

Then, of course, we've got a function that runs if the requests works, and another if it doesn't. Simple `console.log` for now.

Finally, we send our request and see what happens. In this instance, we get back an array of doughnuts.


##AJAX in jQuery

While it's good to have seen the straight old-school Vanilla JS way of doing it, we've got a lot of libraries these days that are designed to get you there faster - jQuery is one of those libraries.

For information about AJAX in jQuery, the best place to go is the [jQuery AJAX Documentation](http://api.jquery.com/category/ajax/).

The main method is called `$.ajax()`, and allows us to specify all the options of our request. However, we most often make _get_ and _post_ requests, so jQuery provides convenience functions for those, where we don't need to provide all the configuration i.e.  `$.get()` and `$.post()`.

**Basic `$.get()` example**

```js
$.get('https://www.reddit.com/search.json', {
  q: 'kittens'
}).done(function(data) {
  console.log(data);
});
```

**This `$.ajax()` example is the same as the `$.get()` example above**

```js
$.ajax({
  url: 'https://www.reddit.com/search.json',
  method: 'GET',
  data: {
    q: 'kittens'
  }
}).done(function(data) {
  console.log(data);
});
```

Note how in the `$.ajax()` example, we need to be more explicit by providing an object with the `url`, `method`, and `data`. Using `$.get()` will assume that the first argument is the URL and the second argument is data we want to send via the query string.

### Forms
We can do GET requests at any point, such as when the page loads, or when our data changes. We can also combine it with a form to create a search.

**HTML**

```html
<form id="search-form">
  <input id="query" placeholder="kittens">
  <button type="submit">search reddit</button>
</form>
```

When the form is submitted, we want to intercept the result and make an AJAX request to the server.

**jQuery / JavaScript**

```js
$("#search-form").on('submit', function (event) {
  // Stop the form from changing the page.
  event.preventDefault();

  // Get the users search input and save it in a variable.
  // Use the input placeholder value (like "kittens") as a default value.
  var input = $("#query");
  var userQuery = input.val() || input.attr("placeholder");

  $.get('https://www.reddit.com/search.json', {
    q: userQuery
  }).done(function(response) {
    console.log(response);
  });
});
```

##AJAX is asynchronous

An AJAX request doesn't allow you to load things instantly. Requesting things over the network always takes time, and you need an event handler to deal with the results. Look at the following code:

```js
console.log('Document is ready');

$.get('https://www.reddit.com/search.json', {
  q: 'kittens'
}).done(function(data) {
  console.log('AJAX is ready');
});

console.log('Just fired AJAX request!');
```

What order will the `console.log` statements appear?

##Promises

Note that at the end of the AJAX request, there is a function called `.done()` that is called once the response has been received. This is an example of a **promise**. Promises are common concepts in JavaScript, and you can think of promises as a "contract" between two functions. When the `.done()` promise is attached to the AJAX function, it "promises" to run once the response comes back successful. This is due to the request-response cycle taking time, and requiring asynchronous behavior.

We can chain additional promises to the AJAX request, and this is a common practice. However, the "contract" still applies. Let's see an example.

```js

$.get('https://www.reddit.com/thispagedoesntexist')
.done(function(data) {
  console.log('This should not run, because a 404 error occurs');
}).fail(function(error) {
  console.log('An error occurred');
});

```

Here, we added a second promise called `.fail()`. Try running the code above by pasting it into the Chrome console on https://www.reddit.com, and see what happens. Only the `.fail()` function runs, and that's because the `.fail()` function makes a promise to the AJAX function that it will only run when there's an error.

Since the `.done()` function made a promise to the AJAX function to only run when successful, the `.done()` function does not run, thus keeping its promise. Keep a lookout for promises implemented in the context of AJAX and other libraries.

##Local Files

We can load local files, such as JSON files, using AJAX/jQuery. All we need to do is use the local file name. As a security precaution, modern browsers will only allow us to do this if we're running a HTTP server. This is to prevent sites from accessing files on our computer that we don't want them to have access to. _Checkout the Installfest section for information on setting up a local http server_

**script.js**

```js
$.getJSON('data.json', function(data) {
  console.log(data);
});
```

**data.json**

```js
{
  "key": "value",
  "number": 5,
  "word": "taco",
  "other": [1, 2, 3, 4, 5]
}
```

The above code would load the data from `data.json` and console.log it. You can put whatever data you want in the data file. This can be useful if you have larger amounts of data that you don't want to hardcode in your js file.

## Cross-Origin Requests

Note that not all websites/APIs play nice with AJAX. You may see an error in the console from APIs like the [iTunes API](https://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html)

```js
$.get('http://itunes.apple.com/search?term=arcade+fire').done(function(data) {
  console.log(data);
});

// output
// > No 'Access-Control-Allow-Origin' header is present on the requested resource.
```

For security reasons, browsers often restrict requests to other websites other than your own. However, some APIs like Reddit and the Peanuts API can have their **servers** configured to allow these types of requests. The browser will know to **not** restrict requests because the server will send back additional information in a **header**.

We can add CORS support to our Express Apps by using the __cors__ package or by manually adding the headers we want to allow in our middleware.

```js
app.use(function (req, res, next) {
  res.header('Access-Control-Allow-Origin', '*')
  res.header('Access-Control-Allow-Headers', 'Origin, X-Requested-With, Content-Type, Accept')
  next()
})
```

## Additional Resources

* [jQuery AJAX Docs](http://api.jquery.com/jquery.ajax/)
* [Some useful jQuery DOM Manipulation Docs](http://api.jquery.com/prepend/)
