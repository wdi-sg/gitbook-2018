# Internet part 3: Servers and Sending HTML over the Internet

## HTTP 102 (Application Layer)
The HT in HTTP: Getting HTML pages over the internet.

---

### Objectives
- learn in-depth the parts of an HTTP request
- learn about the parts of a URL
- review status codes

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

#### The Web Is a Big Collection of HTML Pages on the Internet

We've seen how the network of the internet transports things, but what is it transporting?

The World Wide Web, or "Web" for short, is a massive collection of digital pages: that large software subset of the Internet dedicated to broadcasting content in the form of HTML pages. The Web is viewed by using free software called web browsers.

Born in 1989, the Web is based on hypertext transfer protocol, the language which allows you and me to "jump" (hyperlink) to any other public web page. There are over 65 billion public web pages on the Web today."

- Taken from [About Tech](http://netforbeginners.about.com/od/i/f/What-Is-The-Internet.htm)

---

### Naming of Resources/Pages and Categorization of Data

- Each thing we request is a resource
- URLs are a specficication for the location of a resource
- We format the request for the resources in a particular syntax
- We can request web pages through the browser
- We can request JSON through javascript
- (We can request either through either)
- URLs are formatted similarly to file paths
- There is a root path, ans then a set of resources
- Each web dev chooses a way to format the order of a URL

---

#### Elements of a URL

URL stands for Uniform Resource Locator, and it's just a string of text characters used by Web browsers, email clients and other software to format the contents of an internet request message.

Let's breakdown the contents of a URL:

> Note. Draw the following on the board

```
    http://www.example.org/hello/world/foo.html?foo=bar&baz=bat#footer
    \___/  \_____________/ \__________________/ \_____________/ \____/
  protocol  host/domain name        path         querystring     hash/fragment
```


```
    http://www.example.org:3000/hello/world/foo.html?foo=bar&baz=bat#footer
    \__/  \_____________/ \__/\__________________/ \_____________/ \____/
protocol|host/domain name|port|      path         | querystring   | hash/fragment
```

---


Element           | About
------------------|--------
protocol          | the most popular application protocol used on the world wide web is HTTP. Other familiar types of application protocols include FTP, SSH, GIT, FILE, HTTPS
                  |
host/domain name  | the host or domain name is looked up in DNS to find the IP address of the host - the server that's providing the resource. This may include a subdomain, which in it's simplest sense is like a folder on the server. www is actually a subdomain and is often used by default on servers, allowing you to omit it in the URL sometimes.
                  |
path              | web servers can organise resources into what is effectively files in directories; the path indicates to the server which file from which directory the client wants
                  |
querystring       | the client can pass parameters to the server through the querystring (in a GET request method); the server can then use these to customise the response - such as values to filter a search result
                  |
hash/fragment     | the URI fragment is generally used by the client to identify some portion of the content in the response; interestingly, a broken hash will not break the whole link - it isn't the case for the previous elements

<br>
_The Schema above is from [Tuts +](http://code.tutsplus.com/tutorials/http-the-protocol-every-web-developer-must-know-part-1--net-31177)_



---


## Demo: How to send things over the internet: HTTP
Real-life recreation with paper. (One person pretends to be the backend)
- Prepare a request in the browser. Determine which HTTP method / verbs we are using
- Send the request to a server
- server recieves the response
- reads the headers
- looks at the contents of the request
- based on what was requested / what they want to get back, constructs a response
- Ex: `www.google.com/cities-finder/?search=bar&format=json&page=3`
- sends the response
- browser is waiting for the response
- Response has status and status code on the envelope.
- Deal with the server response
- If it's good, display the output to the user
- If it's bad, do nothing, or display an error to the user

---

### ngrok
Circumvent the LAN network

![http://www.conceptdraw.com/How-To-Guide/picture/Computer-and-networks-Local-area-network-diagram.png](http://www.conceptdraw.com/How-To-Guide/picture/Computer-and-networks-Local-area-network-diagram.png)

ngrok is a tool that gives your computer one of the valuable internet IP addresses.

Download it here: [https://ngrok.com/download](https://ngrok.com/download)

`./ngrok http 80`

When you go to the URL they give, it's your computer!
