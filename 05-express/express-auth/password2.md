# Hashing

Hashing is one of the cornerstones of all cryptography.

We can create a number/string that represents the contents of something, without knowing what that thing is- we are translating the contents of something else into a fixed length number.

---

#### Cyphers:
Transform one text into something else:

**Encoding a cipher example:**
a-z == 1-26 ==> aaa == 111
one letter forwards ==> "hello" == "ifmmp"

Hashing is just a mathematically irreversable cipher. We can't get the actual contents of the hash back out.

This is hard because of one of the properties prime numbers.

---


[From wikipedia:](https://en.wikipedia.org/wiki/Cryptographic_hash_function)


The ideal cryptographic hash function has five main properties:

- it is deterministic so the same message always results in the same hash
- it is quick to compute the hash value for any given message
- it is infeasible to generate a message from its hash value except by trying all possible messages
- a small change to a message should change the hash value so extensively that the new hash value appears uncorrelated with the old hash value
- it is infeasible to find two different messages with the same hash value

---

![https://upload.wikimedia.org/wikipedia/commons/thumb/2/2b/Cryptographic_Hash_Function.svg/750px-Cryptographic_Hash_Function.svg.png](https://upload.wikimedia.org/wikipedia/commons/thumb/2/2b/Cryptographic_Hash_Function.svg/750px-Cryptographic_Hash_Function.svg.png)

---


Hashing turns out to be the best way to store a string that you need to know is legitimate, but don't actually care what the string is. (Or you specifially don't want to know what the string is)

Passwords work by storing a hash of the password, then throwing the actual password away.

#### Uses of hashes:

- git
- bitcoin
- file integrity
- ssh / https / encryption
- TOR / darkweb

---

##Password Hashing

For password protection we'll use sha256. sha256 creates highly secure salted passswords. Learn more about sha256: [SHA-2](https://en.wikipedia.org/wiki/SHA-2). Note that sha256 hashes passwords in an extremely secure way. It differs from other hashing methods like MD5 by putting a roadblock in the way between the hash and a hacker (specifically, time). Let's see how this works.

---

To use sha256 in node we need to install / use the sha256 npm module.

**Install sha256**

```
npm install js-sha256
```


**Hash password**

```js
// require it
var sha256 = require('js-sha256');

//example
sha256('The quick brown fox jumps over the lazy dog'); // d7a8fbb307d7809469ca9abcb0082e4f8d5651e46d3cdb762d02d0bf37c9e592
```

---

## Authentication

We can see whether or not a user is authentic, but do we want them to enter their password for each request? What if they need permission for photos whose `src` is embedded in a page?

We can use cookies to mark whether a user has been authenticated or not.

---

## User Records
In order to store the password hash, we will need to introduce the idea of having a user and the associated information stored on the server.

---

### Process:

1. create user:
  1. user makes GET request to server for the register form
  1. user fills out form and submits a POST request
  1. server recieves POST request
    1. hashes password
    1. creates user record
  1. server sets a cookie in the header of the response.
  1. all subsequent requests to the server will have the `logged_in` cookie

1. login ( user is not currently logged in )
  1. user makes GET request to server for the login form
  1. user fills out form and submits
  1. server recieves POST request
    1. gets user record
    1. hashes password
    1. compares hash to what is in the record
    1. if record and hash match, set a cookie in the response header

1. logout ( user is logged in )
  1. user makes POST request for logout
  1. server recieves POST request
    1. server sends clear cookie in response header

### Pairing exercise:
Create a command line app that hashes the argument:

Create a new dir
```
mkdir sha256
```

cd into it
```
cd sha256
```
init a new package json
```
npm init
```
add sha256
```
npm install js-sha256
```
require it at the top of the file
```
const sha256 = require('js-sha256');
```

take an argument and hash it
```
let input = process.argv[2];
let hash = sha256(input);
```

copy/paste that output somewehere, and then write code that compares two hashes:

```
let otherInput = process.argv[2];

if( hash === sha256(otherInput) ){

  console.log("same");
}else{

  console.log("not same");
}
```
