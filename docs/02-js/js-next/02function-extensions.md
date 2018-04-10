# Function Extensions

## What is the feature
#### Default Parameters
Allows a default value(s) of a parameter to be stated in the function

1. How do we use it?
```js
function bankAccount(cash="please put in cash",pin="please key in correct pin") {

function withdraw(amount, enteredPin) {
   if (enteredPin === pin) {
     cash -= amount;
   }
 }

 function balance() {
   console.log(cash);
 }

 //returning functions in an object so we can access them
 return {
   withdraw: withdraw,
   balance: balance
 }
}

// create a new bank account and withdraw 30
var account = bankAccount();
account.withdraw(30, 1234);

// returns 970
account.balance();

// returns 970 again because the pin failed
account.withdraw(30);
account.balance();

// both return return cash="please put in cash",pin="please key in correct pin"
account.pin
account.cash
```

2. What problem does it solve?
Avoid undefined results. Possible less line of codes

3. Show an example ES5 code vs ES6 code
```js
function bankAccount(cash,pin) {
  cash = (typeof cash !== "undefined" ? cash : "please put in cash")
  pin = (typeof pin !== "undefined" ? pin : "please key in correct pin")
  var cash = 1000
  var pin = 1234

function withdraw(amount, enteredPin) {
   if (enteredPin === pin) {
     cash -= amount;
   }
 }

 function balance() {
   console.log(cash);
 }

 //returning functions in an object so we can access them
 return {
   withdraw: withdraw,
   balance: balance
 }
}

// create a new bank account and withdraw 30
var account = bankAccount();
account.withdraw(30, 1234);

// returns 970
account.balance();

// returns 970 again because the pin failed
account.withdraw(30);
account.balance();

// both return return cash="please put in cash",pin="please key in correct pin"
account.pin
account.cash
```

4. Is this the 80% or the 20% i.e. how useful is this feature to us really
80%.

#### Rest & Spread Operators
Operator "..."
The spread operator takes each value of the array and passes them as individual parameters to a method.

Using the spread/rest operator in these situations makes the code much more declarative about what arguments the function expects and how it will use them.

1. How do we use it?


2. What problem does it solve?
Less line of code

3. Show an example ES5 code vs ES6 code
ES5
```js
var countries = ['Moldova', 'Ukraine'];  
var otherCountries = ['USA', 'Japan'];  
countries.push.apply(countries, otherCountries);  
console.log(countries); // => ['Moldova', 'Ukraine', 'USA', 'Japan']
```

ES6
```js
var countries = ['Moldova', 'Ukraine'];  
var otherCountries = ['USA', 'Japan'];  
countries.push(...otherCountries);  
console.log(countries); // => ['Moldova', 'Ukraine', 'USA', 'Japan']
```

4. Is this the 80% or the 20% i.e. how useful is this feature to us really
20%


#### Fat Arrow
Fat Arrow function is a more concise syntax for writing function expressions. They utilize a new token, "=>", that looks like a fat arrow. Arrow functions are anonymous and change the way this binds in functions.

1. How do we use it?


2. What problem does it solve?
Fewer lines of code

3. Show an example ES5 code vs ES6 code
```js
// ES5
var multiply = function(x, y) {
  return x * y
};

// ES6
var multiply = (x, y) => { return x * y };
```

4. Is this the 80% or the 20% i.e. how useful is this feature to us really
20%
