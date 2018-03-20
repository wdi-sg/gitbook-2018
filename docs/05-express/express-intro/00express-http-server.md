# Internet part 3: Servers and Sending HTML over the Internet

## HTTP 102 (Application Layer)
The HT in HTTP: Getting HTML pages over the internet.

---

### Objectives
- learn in-depth the parts of an HTTP request
- learn about the parts of a URL
- review status codes

---

## The World Wide Web vs. The Internet
Let's look one (network) level up to the kinds of things we are requesting over the internet.

---

![](https://upload.wikimedia.org/wikipedia/commons/c/c9/Client-server-model.svg)

---

- request and response cycle between a client and a host
- stateless ( no matter how many times something is requested, we will do the same thing )
- has a request header (we already saw this)
- has a response header (we will be looking at that this week)
- comes back with a status code

<span class="non-slide"></span><span class="non-slide"></span>
<span class="non-slide"></span><span class="non-slide"></span>

HTTP is the most common protocol we'll work with. It stands for "Hyper Text Transfer Protocol", it allows for communication between a variety of different computers and supports a ton of different network configurations. To make this possible, it assumes very little about a particular system, and does not keep state between different message exchanges.

Read this as: "HTTP makes it easy for many different computers to talk to each other."

This makes HTTP a stateless protocol.

Let's define the following vocabulary:

- **Host** - a computer or other device connected to a computer network; host may offer information resources, services, and applications (via computer code!) to users or other computers on the network. Often used interchangeably with the term server.
  - Ex: servers and web services
- **Client** - the requesting program in the client/host relationship; the client initiates an HTTP request message, which is serviced through a HTTP response message in return
  - Ex: browsers, terminals, SQL clients

To sum it up, communication between a host and a client occurs via a request/response pair. The client initiates an HTTP request message, which is serviced through a HTTP response message in return. We will look at this fundamental message-pair in the next section.

Now, we're making this really simple, just to give you the big idea - there are a ton of intricacies that go into getting your request message to the right place and delivering the information you requested.  We'll go into more detail today and over the rest of the course.


_Text From [Tuts +](http://code.tutsplus.com/tutorials/http-the-protocol-every-web-developer-must-know-part-1--net-31177)_

Note that you will have seen HTTPS used on websites, this is the secure version of the http protocol and is used when transmitting sensitive data. It uses encryption and is more secure but is slower.

---

### Responding to a request: Status codes

<span class="non-slide"></span><span class="non-slide"></span>
<span class="non-slide"></span><span class="non-slide"></span>


Once this request reaches the server, then this server will return a response to the request emitter.

HTTP responses are similar to HTTP requests in the way that they are text based and contain HTTP headers and status. Look on the first line above, again - the HTTP response returns the HTTP status code. This code is very useful for developers working with request/response cycles.  

---

The response codes come as three digit numbers and dictate whether a specific HTTP requests has been successfully completed. Responses are grouped in five classes, with the first digit determining the higher-level categorization:

- 1xx Informational
- 2xx Success
- 3xx Redirection
- 4xx Client Error
- 5xx Server Error

---

## Demo: How to send things over the internet: HTTP
Real-life recreation with paper. (One person pretends to be the backend)
- Prepare a request in the browser. Determine which HTTP method / verbs we are using
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


---

## The Web Is a Big Collection of HTML Pages on the Internet

<span class="non-slide"></span><span class="non-slide"></span>
<span class="non-slide"></span><span class="non-slide"></span>

We've seen how the network of the internet transports things, but what is it transporting?

The World Wide Web, or "Web" for short, is a massive collection of digital pages: that large software subset of the Internet dedicated to broadcasting content in the form of HTML pages. The Web is viewed by using free software called web browsers.

Born in 1989, the Web is based on hypertext transfer protocol, the language which allows you and me to "jump" (hyperlink) to any other public web page. There are over 65 billion public web pages on the Web today."

- Taken from [About Tech](http://netforbeginners.about.com/od/i/f/What-Is-The-Internet.htm)

---

#### Simple File Server
The simplest thing we can use is a thing that simply spits files out after we request them using http

We can write a 9 line file that spits those files out:

```
var http = require('http');
var fs = require('fs');
var path = require('path');

http.createServer(function (request, response) {

    console.log("handling request");

    var filePath = '.' + request.url;

    fs.readFile(filePath, function(error, content) {
        response.end(content, 'utf-8');
    });

}).listen(8125);
console.log('Server running at http://127.0.0.1:8125/');
```
Full exmaple here: [https://developer.mozilla.org/en-US/docs/Learn/Server-side/Node_server_without_framework](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Node_server_without_framework)

---
#### Let's create and run a server:

Make a new dir:
```
mkdir simple-http
```
```
cd simple-http
```
```
touch index.js
```
Copy and paste the code. Save it.
Make a text file.
```
echo "Make Up Your Own Text Here" >> simple.txt
```
Start our server.
```
node index.js
```
Try it on your own browser:
```
http://127.0.0.1:8125/simple.txt
```

---

### Fully Featured npm Server:
Implements HTTP error codes:

`npm install -g serve`

`serve`

What happens when we try to get a file that doesn't exist?

---

### ngrok
Circumvent the LAN network

![http://www.conceptdraw.com/How-To-Guide/picture/Computer-and-networks-Local-area-network-diagram.png](http://www.conceptdraw.com/How-To-Guide/picture/Computer-and-networks-Local-area-network-diagram.png)

---

ngrok is a tool that gives your computer one of the valuable internet IP addresses.

Download it here: [https://ngrok.com/download](https://ngrok.com/download)

`./ngrok http 8125`

When you go to the URL they give, it's your computer!

---

#### Pairing Exercises:
- repeat the above steps on your computer
- install and start ngrok
- slack someone else in the class a link to a file that you created
- text message one of your friends the link and have them open it
- find an html file that you have, copy it into your server directory, and try that in your browser

- try to install `serve` package and run it
- if you get done with that, try to implement features of the full server as specified in the MDN link above:

- Detect and write a nice 404 message to the user who tries to get something that doesn't exist.

- Enable your server to send image files.
