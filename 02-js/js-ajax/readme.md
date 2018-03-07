# The Internet

---

### Context

Before we can describe the internet from a javascript perspective, we have to talk about what the internet is.

- network of computers
- sending things across the network
- just computers talking to computers

---

#### Objectives
- Learn about how things get transported over the internet
- Learn about the pricipal of request response
- See some different responses after requesting things over the internet
- Implement a request and deal with the response in javascript

---

## Demo: How to send things over the internet:
Real-life recreation with paper. (One person pretends to be the backend)
- Prepare a request in the browser
- Send the request to a server
- Wait for the response
- Response has status and status code on the envelope.
- Deal with the server response
- If it's good, display the output to the user
- If it's bad, do nothing, or display an error to the user

---

### Demo: Actually Getting Things
1. Open your browser
1. Type in google.com in the address bar
1. Hit enter (You request the page from google.com)
1. Your browser recieves the response and displays the result

#### Dev Console Network Tab
- clicking on the network tab, you will be able to see all other requests the browser makes when loading a page.

---

## HTTP
- a **structured** protocol for sending and recieving HTML (or other data)
- protocol, as in the military, a set of conventions for behavior given a set of circumstances
- how to deal with scenarios that go wrong: https://swapi.co/lkjlkjlkj
- we are using `GET` method to **get** things. We'll learn others in Unit 2.
- HTTP status codes: [https://en.wikipedia.org/wiki/List_of_HTTP_status_codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)

---

#### Anti-demo: Not getting things
1. Open your browser
1. Type in google.com/lkjlkjlkjljlkjl in the address bar
1. Hit enter (You request the page from google.com)
1. Your browser recieves the response and displays the result

---

### In Javascript

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
request.open("GET", "https://swapi.co/api/people/1");

// send the request
request.send();
```
---

Change it from a string type into a javascript object

```
var responseHandler = function() {
  console.log("response text", this.responseText);
  var response = JSON.parse( this.responseText );
  console.log( response );
};
```

---

- Note: `new` keyword creates a new `XMLHttpRequest`
- Note: `JSON` is built-in functionality that deals with JSON.
- Note: I can also visit https://swapi.co/api/people/1 in the browser.

From: [MDN](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest)
From: [Moar MDN](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/Synchronous_and_Asynchronous_Requests)
https://github.com/toddmotto/public-apis

---

- why do we need to use callbacks?
- we don't want to freeze the user experience while the user loads a slow request

---

### Handling Errors

#### Request "error" event and handlers
```
var requestFailed = function(){
  console.log("response text", this.responseText);
  console.log("status text", this.statusText);
  console.log("status code", this.status);
};

request.addEventListener("error", requestFailed);
```
---

### Pairing Exercises
- Create a new `index.html` file with an empty `body` tag.
- Create an empty `script.js` file
- Put the above code in a `window.onload` function callback.
- Run the above code. See the response on the console.
- Click the network tab and find your ajax request. Click it to see the response body.

#### Further:
- Use this html: `<input id="url" type="text"/><button id="submit">submit</button>` to get user input
- Use this javascript to get the user input:
```
var doSubmit = function(event){
  var input = document.querySelector('#url');
  var url = input.value;
};

document.querySelector('#submit').addEventListener('click', doSubmit);
```
- set the `url` variable as the variable of the request
- check out the swapi [documentation](https://swapi.co/documentation)
- input a few more urls and send a request. See the response in the console.

#### Further 2:
- put in a url string that doesn't make any sense.
- use an `error` event handler callback to alert the user when an error occurs.

#### Further 3:
- make a request to `https://www.google.com/teapot`
- what is the status code and status text?
