## Functions:

### 1.1 Functions basics
1.1.1
  - create a function named hello that prints out 'Hello World'
  - call that function
  
Example:
```
function example() {
  \\ your code goes here
};

example();
```
or:

```
var example = function() {
  \\ your code goes here
};

example();
```

1.1.2
  - create a function that takes in a string as a parameter and prints it

1.1.3
  - create a function that takes in 2 numbers as parameters and returns the product of the 2 numbers
  - add a conditional statement such that multiply will only happen if at least one of the numbers is greater than 0
  - add a conditional statement such that if the multiply does not happen, print 'Criteria not met'
  
1.1.4
  - create a function that takes in an array of numbers as a parameter and prints the odd numbers

1.1.5
  - create a function that converts a string into an array
  - sort the letters in alphabetical order
  - convert the array back into a string
  
1.1.6

  given an array:
  
  ```
  var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]
  ```
  - create a function that takes in an array of numbers as a parameter:
      - interates through all the elements in the array
      - prints 'fizz' if a number is divisible by 3
      - prints 'buzz' if a number is divisible by 5
      - prints 'fizzbuzz' if a number is divisible by 3 and 5
      - prints the number if the above conditions are not met
  - instead of printing each individual result, return a new array of all the results

### 1.2 Callback functions

1.2.1
  
  - create a function which has a callback as a parameter
  - call the hello function (from 1.1.1) with this function
  
  example:
  
  ```
  function example (callback) {
    callback()
  };
  
  example(hello);
  ```

1.2.2

  - Use setInterval to call the hello function (from 1.1.1) every 2 seconds
  
1.2.3
  
  - Use setTimeout to call the hello function (from 1.1.1) after 3 seconds
  
1.2.4
  
  - Given the following:
  
  ```
  var count = 0;
  var start;

function counter () {
// your code here
};

function timer () {
  start = setInterval (counter, 1000)
};

timer();
```
  - write a function counter which will:
    - increment count by 1 and print it
    - clearInterval after the count hits 5 

1.2.5

  - Write a program with the same functionality as above 1.2.4 using setTimeout instead of setInterval
  - hint: you will need to call the timer function within the counter function to continue the count
  
1.2.6
  
  - create a function that takes in an array and a callback as parameters
  
    given an array:
  
  ```
  var arr = [2, 3]
  ```
  
  - pass the array and multiply function (see 1.1.3) as parameters into this function to return the product of the 2 array         elements
