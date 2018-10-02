# Browser Cookies
![https://media.giphy.com/media/EKUvB9uFnm2Xe/giphy.gif](https://media.giphy.com/media/EKUvB9uFnm2Xe/giphy.gif)

---


### Browser Side Persistence
HTTP is a stateless protocol- we don't know anything about what the user has requested previously. How can we keep track of out users?

Cookies.

A cookie is a piece if information we tell the computer to store on the browser.

They come to the browser in the header of a response.

They are (in theory) siloed by domain.

---

#### Demo: Request and Response with Cookies
Real-life recreation with paper. (One person pretends to be the backend)
- Prepare a request in the browser. Determine which HTTP method / verbs we are using
- Send the request to a server
- server recieves the response
- reads the headers
- prepares the response
- sets some cookies in the header
- sends the response w/ headers
- browser recieves the request
- browser looks at the cookies set headers and stores those cookies as keys and values
- every subsequent request to the domain, browser includes those cookies

## express cookie implemenation

Install and include the cookie parser library.
```
npm install cookie-parser
```
```
const cookieParser = require('cookie-parser')
```

Set the configuration to tell express to use the cookie parser.
```
app.use(cookieParser());
```

Inside a GET route set some cookies
```
let visits = 1;
// set cookie
response.cookie('visits', visits);
```
See it work in the browser

---

### Set and change the cookies
#### Implementation: How many times have you visited the site??
```
// get the currently set cookie
var visits = request.cookies['visits'];

// see if there is a cookie
if( visits === undefined ){

  // set a default value if it doesn't exist
  visits = 1;
}else{

  // if a cookie exists, make a value thats 1 bigger
  visits = parseInt( visits ) + 1;
}

// set the cookie
response.cookie('visits', visits);
```

---

### Pairing Exercise

##### part 1

Pick a site and go to it in your browser.
Look at some cookies for a site.
Erase some cookies in your browser.
Make a new request to that page.
Look at your network tab. What cookies are in the response header, that the site is telling your browser to store.

---

##### part 2

Create a new express app. Add express cookie parser library to the express app.

```
mkdir cookies
cd cookies
touch index.js
```

index.js
```
const express = require('express');
const app = express();

app.get('/', (request, response) => {
  // send response with some data (a string)
  response.send(request.path);
});

app.listen(3000, () => console.log('~~~ Tuning in to the waves of port 3000 ~~~'));
```

Add the libraries
```
npm init
npm install express cookie-parser
```

Follow the instructions above to implement cookies.
