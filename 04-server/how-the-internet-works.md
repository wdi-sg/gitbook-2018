# How the Internet works

---

### Context
We already saw how a request gets made over the network, from our browser, but now we are going to see it from the other side, from the server. We will add in a few details we will need to implement in order for our programs to work properly.

---


### Objectives
*After this lesson, students will be able to:*

- Explain the differences between a client and server
- Explain the significance of an Internet Protocol (IP) address
- Explain how data travels through the internet
- Breakdown the components of a URL: protocol, subdomain, domain, extension, resources
- Identify and describe the most common HTTP request/response headers and the information associated with each

---

#### More context: Internet Layers

---
![https://www.webopedia.com/imagesvr_ce/8023/7-layers-of-osi-icon.jpg](https://www.webopedia.com/imagesvr_ce/8023/7-layers-of-osi-icon.jpg)

---

![http://www.escotal.com/Images/Network%20parts/osi.gif](http://www.escotal.com/Images/Network%20parts/osi.gif)

---

Here's what we'll actually be dealing with:
![Imgur](https://i.imgur.com/gbYZ6Oj.gif)

---
#### The Internet is a Big Collection of Computers and Cables

The Internet is named for "interconnection of computer networks". It is a massive hardware combination of millions of personal, business, and governmental computers, all connected like roads and highways.

No single person owns the Internet. No single government has authority over its operations. Some technical rules and hardware/software standards enforce how people plug into the Internet, but for the most part, the Internet is a free and open broadcast medium of hardware networking.


---

### HTTP 101 (Application Layer)

---

![](https://upload.wikimedia.org/wikipedia/commons/c/c9/Client-server-model.svg)

---
- request and response cycle between a client and a host
- stateless ( no matter how many times something is requested, we will do the same thing )
- has a request header (we already saw this)
- has a response header (we will be looking at that this week)
- comes back with a status code

# _
# _
# _
# _
# _
# _
# _

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

### Responding to a request


Once this request reaches the server, then this server will return a response to the request emitter.

HTTP responses are similar to HTTP requests in the way that they are text based and contain HTTP headers and status. Look on the first line above, again - the HTTP response returns the HTTP status code. This code is very useful for developers working with request/response cycles.  

They come as three digit numbers and dictate whether a specific HTTP requests has been successfully completed. Responses are grouped in five classes, with the first digit determining the higher-level categorization:

- 1xx Informational
- 2xx Success
- 3xx Redirection
- 4xx Client Error
- 5xx Server Error

---

## Demo: How to send things over the internet:
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

#### How to reach a specific server

All computers on the internet have a unique numeric address. This is the way computers find "each other" when communicating. You may recognize the format below - it's an IP address:

```
123.123.123.123
```

Traditionally, IP addresses are composed of four bytes of data separated by periods. Since four bytes *only* offer around 4.3 billion unique IP addresses, they've all but run out. A migration is underway to IPv6 that uses 16 bytes - or 4.3 billion to the power of 4 - that would provide an absurd 42 undecillion unique combinations.

IPv6 addresses are represented as a colon-separated chain of four base-16 digits. For convenience groups of zeroes are squashed. IPv4 addresses neatly fit inside IPv6 addresses.

```
2001:0db8:85a3:0000:0000:8a2e:0370:7334
2001:0db8:85a3::8a2e:0370:7334
::ffff:192.0.2.128
```

---

#### IP Addresses to domains (Internet Layer)

Of course, these numbers are hard to remember, and are not really human-friendly. Can you imagine if website addresses had to be given this way? Instead of commercials on the radio saying "go to OurWebsite.com", they'd be saying "go to 12.14.142.231" and repeating it five times just to make sure everyone got it!

So what was needed was a way to translate real names in to those numbers. This is why we have domain registrars - basically, you reserve the name, and at the domain registrar, your domain name is pointed to the server's particular unique address.

When you type a website domain in to your web browser (or other internet capable app), what happens is a "DNS Lookup" of that particular domain's IP address, so your computer can actually connect to it.  DNS stands for "Domain Name System", and it's the way that Internet domain names are located and translated into Internet Protocol addresses.

| Domain Name  | IP Address    |
|--------------|---------------|
| apple.com    | 17.172.224.47 |
| facebook.com | 31.13.65.1    |
| google.com   | 216.58.192.46 |

So when you go to apple.com, your browser asks a DNS server "what is the IP address of apple.com?" The DNS server responds with "17.172.224.47", and the browser can then connect to 17.172.224.47.



---

#### How DNS Servers Look Up IP Addresses

Your computer looks up IP addresses for domains by asking the nearest DNS server - typically, the local wifi router. The wifi router will not know the IP address unless it was previously looked up, so, the router will further up the hierarchy to your internet service provider's DNS server.

Often, another ISP customer has already requested the domain lookup and the IP address is cached for a while, allowing a quick response.

Otherwise, the lookup is escalated to the top level domain (TLD) registry that has a list of registrars per domain. The registrar is the final authority that resolves the IP address.

The response is passed back along the chain and cached at each step to speed up future lookups.

> Note: The caching along the chain is great for performance. However, changing the IP address of a domain name will not propagate to all visitors until cached lookups expire - typically 24hrs.

Use the command line to get an IP address:
```
host google.com
```

---

## DNS Demo: How to send things over the internet:
Real-life recreation with paper. (One person pretends to be the backend)
- Prepare a request in the browser. Determine which HTTP method / verbs we are using
- send a request to the DNS server to translate the domain name to an IP
- receive the DNS response
- Send the request to the IP indicated
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

#### Ports (Transport Layer)

Ports are the channels that a server agrees to listen to and hence where a client must send their messages and wait for their responses. Each port represents a "process" or execution context for an application on the CPU.

Ports work like the identifying apartment number / floor of a large building. You add them onto the address of the building itself in order to go exactly where you want.

Some port usages are agreed upon by everyone, and therefore you don't have to specify the port. This is why `google.com` still works without specifying port 80.

Some well known ports:
```
80: all http requests
443: all SSL traffic
54: DNS
```

## Demo: How to send things over the internet: http version
Real-life recreation with paper. (One person pretends to be the backend)
- Prepare a request in the browser. Determine which HTTP method / verbs we are using
- Send the request to a server on port 80
- server recieves the response on port 80
- reads the headers
- looks at the contents of the request
- constructs a response
- sends the response on port 80
- browser is waiting for the response
- Response has status and status code on the envelope.
- Deal with the server response
- If it's good, display the output to the user
- If it's bad, do nothing, or display an error to the user

## Demo: How to send things over the internet: http version
Real-life recreation with paper. (One person pretends to be the backend)
- Prepare a request in the browser. Determine which HTTP method / verbs we are using
- Send the request to a server on port 443
- server recieves the response on port 443
- reads the headers
- looks at the contents of the request
- constructs a response
- sends the response on port 443
- browser is waiting for the response
- Response has status and status code on the envelope.
- Deal with the server response
- If it's good, display the output to the user
- If it's bad, do nothing, or display an error to the user


---
#### Use Traceroute To Track Internet Routing

![http://en.dnstools.ch/visual-traceroute.html](http://en.dnstools.ch/visual-traceroute.html)

- at the network level each request and response is broken up into packets. Those packets are routed across the network in order to eventually arrive at their destination. (The IP address we got from the DNS server)

---


## TCP Demo: How to send things over the internet:
Real-life recreation with paper. (One person pretends to be the backend)
- Prepare a request in the browser. Determine which HTTP method / verbs we are using
- Our operating system-level network functionality divides our message into packets
- Send the request packets through the network
- server recieves the response
- reads the headers
- looks at the contents of the request
- constructs a response
- the operating system network functionality divides the response into packets
- sends the response packets
- packets arrive at the client and are reconstructed
- browser is waiting for the response
- Response has status and status code on the envelope.
- Deal with the server response
- If it's good, display the output to the user
- If it's bad, do nothing, or display an error to the user


---

## The World Wide Web vs. The Internet
Let's look one level up to the kinds of things we are requesting over the internet.

#### The Web Is a Big Collection of HTML Pages on the Internet

We've seen how the network of the internet transports things, but what is it transporting?

The World Wide Web, or "Web" for short, is a massive collection of digital pages: that large software subset of the Internet dedicated to broadcasting content in the form of HTML pages. The Web is viewed by using free software called web browsers.

Born in 1989, the Web is based on hypertext transfer protocol, the language which allows you and me to "jump" (hyperlink) to any other public web page. There are over 65 billion public web pages on the Web today."

- Taken from [About Tech](http://netforbeginners.about.com/od/i/f/What-Is-The-Internet.htm)

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

Exercises:
