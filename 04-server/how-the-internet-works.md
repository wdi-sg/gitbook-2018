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
![http://1.bp.blogspot.com/-VU8FEIDWXlc/Tk-hPTxzDdI/AAAAAAAAAHw/UGDZ2hFx4PQ/s1600/memes-the-internet-is.jpg](http://1.bp.blogspot.com/-VU8FEIDWXlc/Tk-hPTxzDdI/AAAAAAAAAHw/UGDZ2hFx4PQ/s1600/memes-the-internet-is.jpg)

<span class="non-slide"></span>

The Internet is named for "interconnection of computer networks". It is a massive hardware combination of millions of personal, business, and governmental computers, all connected like roads and highways.

No single person owns the Internet. No single government has authority over its operations. Some technical rules and hardware/software standards enforce how people plug into the Internet, but for the most part, the Internet is a free and open broadcast medium of hardware networking.


---

#### How to reach a specific server

All computers on the internet have a unique numeric address. This is the way computers find "each other" when communicating. You may recognize the format below - it's an IP address:

```
123.123.123.123
```
---

### IPv6
Traditionally, IP addresses are composed of four bytes of data separated by periods. Since four bytes *only* offer around 4.3 billion unique IP addresses, they've all but run out. A migration is underway to IPv6 that uses 16 bytes - or 4.3 billion to the power of 4 - that would provide an absurd 42 undecillion unique combinations.

IPv6 addresses are represented as a colon-separated chain of four base-16 digits. For convenience groups of zeroes are squashed. IPv4 addresses neatly fit inside IPv6 addresses.

```
2001:0db8:85a3:0000:0000:8a2e:0370:7334
2001:0db8:85a3::8a2e:0370:7334
::ffff:192.0.2.128
```

---

![http://www.conceptdraw.com/How-To-Guide/picture/Computer-and-networks-Local-area-network-diagram.png](http://www.conceptdraw.com/How-To-Guide/picture/Computer-and-networks-Local-area-network-diagram.png)

---
http://www.conceptdraw.com/How-To-Guide/picture/Computer-and-networks-Local-area-network-diagram.png

#### IP Addresses to domains (Internet Layer)
<span class="non-slide"></span>
<span class="non-slide"></span>

Of course, these numbers are hard to remember, and are not really human-friendly. Can you imagine if website addresses had to be given this way? Instead of commercials on the radio saying "go to OurWebsite.com", they'd be saying "go to 12.14.142.231" and repeating it five times just to make sure everyone got it!

So what was needed was a way to translate real names in to those numbers. This is why we have domain registrars - basically, you reserve the name, and at the domain registrar, your domain name is pointed to the server's particular unique address.

When you type a website domain in to your web browser (or other internet capable app), what happens is a "DNS Lookup" of that particular domain's IP address, so your computer can actually connect to it.  DNS stands for "Domain Name System", and it's the way that Internet domain names are located and translated into Internet Protocol addresses.

---

| Domain Name  | IP Address    |
|--------------|---------------|
| apple.com    | 17.172.224.47 |
| facebook.com | 31.13.65.1    |
| google.com   | 216.58.192.46 |

So when you go to apple.com, your browser asks a DNS server "what is the IP address of apple.com?" The DNS server responds with "17.172.224.47", and the browser can then connect to 17.172.224.47.



---

#### How DNS Servers Look Up IP Addresses
<span class="non-slide"></span>
<span class="non-slide"></span>


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
![http://www.aiatopten.org/sites/default/files/styles/popup/public/RCA%20Awards%20Drawings12.jpg](http://www.aiatopten.org/sites/default/files/styles/popup/public/RCA%20Awards%20Drawings12.jpg)

---

Ports are the channels that a server agrees to listen to and hence where a client must send their messages and wait for their responses. Each port represents a "process" or execution context for an application on the CPU.

Ports work like the identifying apartment number / floor of a large building. You add them onto the address of the building itself in order to go exactly where you want.

Some port usages are agreed upon by everyone, and therefore you don't have to specify the port. This is why `google.com` still works without specifying port 80.

Some well known ports:
```
80: all http requests
443: all SSL traffic
54: DNS
```

---

## Demo: How to send things over the internet: ports with http version
Real-life recreation with paper. (One desk is a computer. One person at the desk person pretends to be the backend)


#### Use Traceroute To Track Internet Routing

[http://en.dnstools.ch/visual-traceroute.html](http://en.dnstools.ch/visual-traceroute.html)

- at the network level each request and response is broken up into packets. Those packets are routed across the network in order to eventually arrive at their destination. (The IP address we got from the DNS server)

---


## TCP Demo: How to send things over the internet:
Real-life recreation with paper. (One person pretends to be the backend)
