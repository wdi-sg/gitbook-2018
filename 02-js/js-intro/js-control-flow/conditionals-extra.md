## 1. conditionals:
  - create a variable `monkey` that creates a random number from 1 to 10 (just copy and paste this code): `var monkey = Math.floor(Math.random() * Math.floor(10));`
  - for each conditional you write, also console.log the word `chocolate` after the conditional statement (closing curly brace).
Example:
```
if( 3 == 4 ){

}
console.log("chocolate");
```
  - write a conditional that will `console.log` `hello` if `monkey` is less than 6
  - write a conditional that will `console.log` `goodbye` if `monkey` is 6 or is more than six.
  - write a conditional with an else statement- `console.log` `cats` if `monkey` is less than 3, otherwise `console.log` `goats`
  - write a conditional with an else statement- `console.log` `almonds` if `monkey` is less than 9, otherwise `console.log` `jelly`

  - create another variable `johnathan` that creates a random number from 1 to 10 (just copy and paste this code): `var johnathan = Math.floor(Math.random() * Math.floor(10));`

  - write a conditional that will `console.log` `dollars` if `monkey` is 6 or less than 6 and `johnathan` is less than 5
  - write a conditional with an else statement- `console.log` `dogs` if `monkey` is 3 and `johnathan` is less than 5, otherwise `console.log` `submarine`
  - write a conditional with an else statement- `console.log` `puppies` if `monkey` is more than 7 or `johnathan` is less than 3, otherwise `console.log` `godzilla`
  - write a conditional that will `console.log` `halifax` if `monkey` is 6 or less than 6 and `johnathan` is less than 5 and `johnathan` is not 3
  - write a conditional that will `console.log` `halifax` if `monkey` is 6 or less than 6 and `johnathan` is less than 5 and `johnathan` is not 3

  - write a conditional with an if else statement- `console.log` `electric` if `monkey` is less than 3, but, if `monkey` is equal to 5, `console.log` `camper van`
  - write a conditional with an if else statement- `console.log` `toes` if `monkey` is more than 3, but, if `monkey` is equal to 1, `console.log` `shoelace`
  - write a conditional with an if else statement- `console.log` `curry` if `monkey` is more than 9, but, if `monkey` is equal to 4, `console.log` `future`. Otherwise `console.log` `orchid`.

  - write a conditional that will `console.log` `danube` if `monkey` is less than 6 or `johnathan` is less than 3, but, if `monkey` is more than 7 and `johnathan` is equal to 9, `console.log` `trumpet`. Otherwise `console.log` `brains`

## 2. loops

### 2.1.0 while loop

2.1.1
  - initialize a variable `i`
  - write a while loop that:
    - the condition is: i is less than 12
    - every iteration of the loop `i` increases by `1`
    - every iteration of the loop `console.log` `i` with a string: `console.log("value of i", i);`
2.1.2

  - initialize a variable `i`
  - write a while loop that:
    - the condition is: i is less than 40
    - every iteration of the loop `i` increases by `3`
    - every iteration of the loop `console.log` `i` with a string: `console.log("value of i", i);`

2.1.3

  - initialize a variable `i`
  - write a while loop that:
    - the condition is: i is more than 400
    - every iteration of the loop `i` decreases by `8`
    - every iteration of the loop `console.log` `i` with a string: `console.log("value of i", i);`

2.1.4

  - initialize a variable `i`
  - write a while loop that:
    - for every iteration of the loop, create a variable `monkey` that creates a random number from 1 to 10 (just copy and paste this code): `var monkey = Math.floor(Math.random() * Math.floor(10));`
    - the condition is: i is more than 400
    - every iteration of the loop `i` decreases by `monkey`
    - every iteration of the loop `console.log` `i` with a string: `console.log("value of i", i);`
    - every iteration of the loop `console.log` `monkey` with a string: `console.log("value of monkey", monkey);`


2.1.5
  - create a variable `monkey` that creates a random number from 1 to 10 (just copy and paste this code): `var monkey = Math.floor(Math.random() * Math.floor(10));`
  - write a while loop that:
    - stops if `monkey` is 9 or more
    - every iteration of the loop `monkey` increases by 1

2.1.6

  - initialize a variable `i`
  - given this array:
  ```
  var vegetables = [
    'carrots',
    'celery',
    'potatoes',
    'onions',
    'garlic',
    'daikon'
  ];
  ```
  - write a while loop that:

    - `console.log` the value of each element of the array
    - stops after each element of the array has been console logged
    - the condition is: `i` is less than `6`
    - every iteration of the loop `i` increases by `1`
    - every iteration of the loop `console.log` `i` with a string: `console.log("value of i", i);`
    - every iteration of the loop `console.log` the value at an index of the array- with a string: `console.log("value of vegetables at the index i", i, vegetables[i]);`


2.1.7
  - initialize a variable `i`
  - given this array:
  ```
  var vegetables = [
    'carrots',
    'celery',
    'potatoes',
    'onions',
    'garlic',
    'daikon'
  ];
  ```
  - write a while loop that:

    - `console.log` the value of each element of the array
    - stops after each element of the array has been console logged
    - the condition is: `i` is less than the length of the array: `vegetables.length`
    - every iteration of the loop `i` increases by `1`
    - every iteration of the loop `console.log` `i` with a string: `console.log("value of i", i);`
    - every iteration of the loop `console.log` the value at an index of the array- with a string: `console.log("value of vegetables at the index i", i, vegetables[i]);`


2.1.8
  - initialize a variable `i`
  - given this array:
  ```
  var vegetables = [
    'carrots',
    'celery',
    'potatoes',
    'onions',
    'garlic',
    'daikon'
  ];
  ```
  - write a while loop that:
    - `console.log` the value of each element of the array
    - stops after each element of the array has been console logged


2.1.9
  - initialize a variable `i`
  - given this array:
  ```
  var vegetables = [
    'carrots',
    'celery',
    'potatoes',
    'onions',
    'garlic',
    'daikon'
  ];
  ```
  - write a while loop that:
    - `console.log` the value of an element of the array at `i`
    - every iteration of the loop `i` increases by `2`
    - stops after each element of the array has been console logged

2.1.10

  - initialize a variable `i`
  - given this array:
  ```
  var vegetables = [
    'carrots',
    'celery',
    'potatoes',
    'onions',
    'garlic',
    'daikon'
  ];
  ```
  - write a while loop that:
    - `console.log` the value of an element of the array at `i`
    - every iteration of the loop `i` increases by `3`
    - stops after each element of the array has been console logged

2.1.11
  - initialize a variable `i`
  - given this array:
  ```
  var vegetables = [
    'carrots',
    'celery',
    'potatoes',
    'onions',
    'garlic',
    'daikon'
  ];
  ```
  - write a while loop that:
    - `console.log` the value of each element of the array
    - stops after each element of the array has been console logged
    - write a conditional that runs for each iteration of the loop:
      - if `i` is equal to `3`, print "hooray"


2.1.12
  - create a variable `monkey` that creates a random number from 1 to 10 (just copy and paste this code): `var monkey = Math.floor(Math.random() * Math.floor(10));`
  - write a while loop that:
    - stops if `monkey` is 9 or more
    - every iteration of the loop `monkey` increases by 1
    - write a conditional that runs for each iteration of the loop:
      - if `i` is divisible to `3`, print "hooray"

2.1.13

  - create a variable `monkey` that creates a random number from 1 to 100 (just copy and paste this code): `var monkey = Math.floor(Math.random() * Math.floor(100));`
  - write a while loop that:
    - stops if `monkey` is 9 or more
    - every iteration of the loop `monkey` increases by 1
    - write a conditional that runs for each iteration of the loop:
      - if `i` is divisible to `3`, print "hooray"
      - if `i` is divisible to `5`, print "oh no!"


### 2.2.0 for loop
  - recreate each of these problems using a `for` loop and not a while loop

### 2.3.0 for...in loop

2.3.1
  - given this object:
  ```
  var vegetables = {
    'carrots': 2,
    'celery': 4,
    'potatoes': 5,
    'onions': 1,
    'garlic': 7,
    'daikon': 3
  };
  ```
  - write a for...in loop that:
    - `console.log` all the property keys (i.e. the vegetable types) of the object
    - `console.log` all the property values (i.e. the quantity of each type of vegetable) of the object

2.3.2
   - with the same object above in 2.3.1
   - write a for...in loop that:
     - using a conditional, print 'carrots found' if 'carrots' exists in the object

2.3.3
   - with the same object above in 2.3.1
   - write a for...in loop that:
     - using a conditional, prints all vegetable types with even number quantities
     - using a conditional, prints all vegetable types with quantity less than or equal to 5
  
2.3.4
   - with the same object above in 2.3.1
   - write a for...in loop that:
     - prints the number of types of vegetables
     - prints the total quantity of vegetables across all types

    
