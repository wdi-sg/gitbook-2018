## Internet Part 4:

Let's add the idea that we want to get specific things over the internet. This is essentially what a web app is. It gets information but formatted in a specific way beyond just spitting out static files. In order to take more specific input, we need to specify what we want in a more complex way. The getting and formatting of this data is what this class is all about.

### Naming of Resources/Pages and Categorization of Data

- Each thing we request is a resource
- URLs are a specficication for the location of a resource
- We format the request for the resources in a particular syntax
- We can request web pages through the browser
- We can request JSON through javascript
- (We can request either through either)
- URLs are formatted similarly to file paths
- There is a root path, and then a set of resources
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
