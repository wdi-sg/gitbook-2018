# Pinging Around the World

#### Use the `host` command to see what the IP address of a host
```
host google.com
```

#### Use the `ping` command to see how long it takes for servers to respond to your computer:

All requests sent from your computer must travel across the internet, going
through various servers, in order to make the delivery. Using the `ping`
command in the terminal window, try to find servers that respond quickly, and
find some that take longer to respond. Hit `CTRL + C` if you need to exit
`ping`. **What are the fastest and slowest sites you can ping?**

> University servers are usually hosted on their actual campuses. Use
> university servers to estimate how long it takes requests to travel around
> the world. Look up the distance between cities and calculate kilometers / time in
> milliseconds to calculate how fast these requests travel.

```
# Less than 10 km away from GA Singapore campus
ping www.nus.edu.sg
```

##### National University of Singapore
```
PING www.nus.edu.sg (137.132.21.27): 56 data bytes
64 bytes from 137.132.21.27: icmp_seq=0 ttl=42 time=5.442 ms
64 bytes from 137.132.21.27: icmp_seq=1 ttl=42 time=6.124 ms
64 bytes from 137.132.21.27: icmp_seq=2 ttl=42 time=5.114 ms
^C
--- www.nus.edu.sg ping statistics ---
3 packets transmitted, 3 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 5.114/5.560/6.124/0.421 ms
```

##### Stanford University in California
```
PING stanford.edu (171.67.215.200): 56 data bytes
64 bytes from 171.67.215.200: icmp_seq=0 ttl=235 time=173.482 ms
64 bytes from 171.67.215.200: icmp_seq=1 ttl=235 time=201.556 ms
64 bytes from 171.67.215.200: icmp_seq=2 ttl=237 time=186.975 ms
^C
--- stanford.edu ping statistics ---
3 packets transmitted, 3 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 173.482/187.338/201.556/11.464 ms
```

##### Other World Wide Campuses

Try pinging these other campuses around the world.

```
$ ping www.u-tokyo.ac.jp # The University of Tokyo: 5,312 km from Singapore, in the other direction!
$ ping www.cam.ac.uk # Cambridge University in London: 10,841 km from Singapore
$ ping sydney.edu.au # The University of Sydney in Australia: 7,822 km from Singapore
$ ping www.hbc.ac.za # Helderberg College in Cape Town, South Africa: 9,661 km from Singapore
$ ping www.cs.nyu.edu # New York University: 15,323 km from Singapore
$ ping www.uce.edu.ec # Central University of Ecuador in South America: 19,774 km from Singapore
```

##### Mystery IP Address

Ping this specific IP address and see how long it takes the server to respond.

Google this IP to find out what makes it unique. Be sure to use the Google
homepage. IP addresses are hard to quick-search from your browser's location
bar! If you type in an IP address your browser will try to load it as if it
were a web page!

```
ping 127.0.0.1
```

#### Experiment with the traceroute command to see how internet traffic flows between your computer and servers:


Use visual traceroute to see on a map where your packets are going.
[http://en.dnstools.ch/visual-traceroute.html](http://en.dnstools.ch/visual-traceroute.html)

```
# the traceroute command will show which servers routed the traffic
traceroute stanford.edu
```

#####Sample Output

```
$ traceroute www.nus.edu.sg 
traceroute to www.nus.edu.sg (137.132.21.27), 64 hops max, 52 byte packets
 1  192.168.85.1 (192.168.85.1)  1.738 ms  1.989 ms  1.157 ms
 2  bb118-200-215-254.singnet.com.sg (118.200.215.254)  3.496 ms  2.870 ms  2.928 ms
 3  202.166.120.58 (202.166.120.58)  4.530 ms  4.225 ms  4.371 ms
 4  202.166.120.57 (202.166.120.57)  2.599 ms  2.401 ms  2.406 ms
 5  xe-9-2-1.tp-ar11.singnet.com.sg (202.166.120.29)  58.050 ms  4.321 ms  2.436 ms
 6  ae8-0.tp-cr03.singnet.com.sg (202.166.122.50)  3.603 ms  6.846 ms  3.853 ms
 7  202.166.120.110 (202.166.120.110)  4.142 ms
    ae6-0.qt-cr03.singnet.com.sg (202.166.120.185)  68.517 ms
    202.166.120.110 (202.166.120.110)  4.784 ms
 8  ae12-0.qt-er05.singnet.com.sg (202.166.120.221)  6.161 ms  3.113 ms  4.599 ms
 9  202.166.126.114 (202.166.126.114)  3.183 ms  3.548 ms  3.916 ms
10  ten-4-1.sn-sintp1-au301.singnet.com.sg (165.21.12.44)  4.497 ms  4.706 ms  3.715 ms
11  118.201.215.82 (118.201.215.82)  4.548 ms  7.821 ms  4.359 ms
12  202.51.240.230 (202.51.240.230)  4.612 ms  23.792 ms  4.559 ms
13  nus-gw1.gigapop.nus.edu.sg (202.51.241.14)  5.183 ms  4.567 ms  4.835 ms
14  * * *
15  *^C
```

#### Experiment with cURL and send requests to various web pages. Here are some
useful flags you can use:

#####Headers flag
```
# -I returns the response headers only
curl -I http://www.google.com
```

#####Sample Output
```
HTTP/1.1 200 OK
Date: Sun, 27 Sep 2015 02:28:12 GMT
Expires: -1
Cache-Control: private, max-age=0
Content-Type: text/html; charset=ISO-8859-1
...
```

#####Verbose mode
```
# -v is verbose mode and returns the entire request and response (along with some additional info)
curl -v http://www.google.com
```

#####Sample Output
```
* Rebuilt URL to: http://www.google.com/
*   Trying 2607:f8b0:400a:806::2004...
* Connected to www.google.com (2607:f8b0:400a:806::2004) port 80 (#0)
...
```

#### Using cURL and the `-I` flag, get the following response codes from some webpages:

- 2xx - examples include 200 (OK), 201 (Created)
- 3xx - examples include 301 (Moved permanently)
- 4xx - examples include 400 (Bad request), 404 (Not found)

