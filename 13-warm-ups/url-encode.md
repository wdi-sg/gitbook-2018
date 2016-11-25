# URL Encoder
Write a function called urlEncode that accepts two parameters:
1. a string representing a URL
2. an optional object containing key value pairs

The function should return a URL with a well-formatted query string.

* If `params` is undefiend simply return the url.
* The url and the beginning of the parameters should be separated with a questions mark `?`
* Every parameter after the first should be separated with an ampersand `&`

### Starter Code
```JavaScript
function urlEncode(url, params) {
  // TODO: implement this function
}

function test(msg, actual, expected) {
  if (actual === expected) {
    console.log("PASSED:", msg);
  } else {
    console.log("FAILED:", msg);
  }
}

// TEST CASES
// these test cases are provided to clarify the specification and help you test your function:
var msg1 = "It should return the URL as it is passed if there are no params provided.";
test(msg1, urlEncode("www.google.com"), "www.google.com")
test(msg1, "it should return the URL as it is passed if there are no params provided.", urlEncode("www.google.com/"), "www.google.com/")

var msg2 = "It should show a question mark for the first parameter.";
test(msg2, urlEncode("site.com", {user: "Frank"}), "site.com?user=Frank")

var msg3 = "It should separate every other query parameter with an '&' ampersand.";
var params = {user: "Frank", day: "Saturday", id: 14};
test(msg3, urlEncode("site.com", params), "site.com?user=Frank&day=Saturday&id=14")

```
