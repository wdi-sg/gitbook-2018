# Hashing

Hashing is one of the cornerstones of all cryptography.

We can create a number/string that represents the contents of something, without nowing what that thing is- we are translating the contents of something else into a standardized 12 digit number (for example) that has a very, very low chance of representing something else.

---

#### Encoding a cipher example:
a-z == 1-26 ==> aaa == 111
one letter forwards ==> "hello" == "ifmmp"

Hashing is just a mathematically irreversable cipher. We can't get the actual contents of the hash back out. Because prime numbers.


[From wikipedia:](https://en.wikipedia.org/wiki/Cryptographic_hash_function)

---
The ideal cryptographic hash function has five main properties:

- it is deterministic so the same message always results in the same hash
- it is quick to compute the hash value for any given message
- it is infeasible to generate a message from its hash value except by trying all possible messages
- a small change to a message should change the hash value so extensively that the new hash value appears uncorrelated with the old hash value
- it is infeasible to find two different messages with the same hash value

---

![https://upload.wikimedia.org/wikipedia/commons/thumb/2/2b/Cryptographic_Hash_Function.svg/750px-Cryptographic_Hash_Function.svg.png](https://upload.wikimedia.org/wikipedia/commons/thumb/2/2b/Cryptographic_Hash_Function.svg/750px-Cryptographic_Hash_Function.svg.png)


Hashing turns out to be the best way to store a string that you need to know is legitimate, but don't actually care what the string is. (Or you specifially don't want to know what the string is)

Passwords work by storing a hash of the password, then throwing the actual password away.

---

##Password Hashing

For password protection we'll use bcrypt. Bcrypt creates highly secure salted passswords. Learn more about bcrypt: [bcrypt wiki](http://en.wikipedia.org/wiki/Bcrypt). Note that bcrypt hashes passwords in an extremely secure way. It differs from other hashing methods like MD5 by putting a roadblock in the way between the hash and a hacker (specifically, time). Let's see how this works.

To use bcrypt in node we need to install / use the bcrypt npm module.

**Install bcrypt**

```
yarn add bcrypt
```

**Hash password**

```js
// require it
const bcrypt = require('bcrypt');

//example
bcrypt.hash('myPassword', 1, (err, hash) => {
  //hash = hashed password (using salt)
});
```

**bcrypt.hash() takes 3 parameters**

* Password to hash -- self explanitory
* Rounds -- Number of rounds of processing when generating the salt. The higher the number, the longer it takes to generate the hash, and the more secure the hash.
* Callback function (called when the hashing completes)

**Note about rounds:** The higher the number, the longer it will take for a potential hacker to crack the password via brute-force. HOWEVER, it also takes longer to create the password. The default value of 10 takes less than a second. A value of 13 will take about a second. 25 will take about an hour and 30 will take DAYS to complete. The default value of 10 is perfectly fine for now.

[More info about bcrypt module](https://www.npmjs.com/package/bcrypt)

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
  1. user makes get request to server for the register form
  1. user fills out form and submits
  1. server recieves post request
    1. hashes password
    1. creates user record
  1. server sets a cookie in the header of the response.
  1. all subsequent requests to the server will have the `logged_in` cookie

1. login ( user is not currently logged in )
  1. user makes get request to server for the login form
  1. user fills out form and submits
  1. server recieves post request
    1. gets user record
    1. hashes password
    1. compares hash to what is in the record
    1. if record and hash match, set a cookie in the response header

1. logout ( user is logged in )
  1. user makes post request for logout
  1. server recieves post request
    1. server sends clear cookie in response header

### Pairing exercise:
Create a command line app that hashes the argument:

Create a new dir
```
mkdir bcrypt
```

cd into it
```
cd bcrypt
```
init a new package json
```
yarn init
```
add bcrypt
```
yarn add bcrypt
```
require it at the top of the file
```
const bcrypt = require('bcrypt');
```

take an argument and hash it
```
let input = process.argv[2];

bcrypt.hash(input, 1, (err, hash) => {
  console.log( hash );
});
```

copy/paste that output somewehere, and then write code that compares two hashes:

```
bcrypt.compare(input, oldInput, function(err, res) {
  if( res === true ){

    console.log("same");
  }else{

    console.log("not same");
  }
});
```

change the number of rounds:
```
let input = process.argv[2];

bcrypt.hash(input, 100, (err, hash) => {
  console.log( hash );
});
```
