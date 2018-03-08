# Destructuring in ES6
This is part of a series of guides written by students from WDI7 Singapore. Originally written by Olivia, Keith and Larissa.

Destructuring has 2 main uses:

1. Easy declaration of variables by assigning values buried in arrays and objects
2. Declaring default values when assignment fails to find a value

## Destructuring Arrays
1-to-1 mapping helps to easily declare a set of variables and assign them values from an array within a single line, instead of assigning them individually.

In ES5:

```javascript
var list = [1,2,3]
var a = list[0]
var c = list[2]
```

In ES6:

```javascript
let [a, ,c] = [1,2,3]
console.log(a) // 1
console.log(c) // 3
```

A cool thing that destructuring can do is dumping the remaining unassigned elements in an array into a variable:

```javascript
let [a,b, ...c] = [1,2,3,4,5]
console.log(c) // [3,4,5]
```

### Grabbing/assigning from returned arrays

A useful case of deconstructing is when you grab an array that was returned from a function and immediately deconstruct the result in order to save the elements into their own variables:

```javascript
const myFunc = function() {
  return [result1,importantResult,result3]
}

let [a,b,c] = myFunc()
console.log(b) // importantResult
```

### Setting default values

You can also set default values in deconstruction in the event that a variable cannot find a value to be assigned.

```javascript
let [a,b,c='default'] = [1,2]
console.log(c) // 'default'
```

## Destructuring Objects

### Object Assignment with declaration

Similarly named keys will take on the same values and can be referred to directly.

In ES5:

```javascript
var o = {
  p: 42,
  q: true
}
var p = o.p
var q = o.q

console.log(p) // 42
console.log(q) // true
```

In ES6:

```javascript
let o = {p: 42, q: true}
let {p, q} = o

console.log(p) // 42
console.log(q) // true
```

### Object Assignment without declaration

A variable can be assigned its value with destructuring separate from its declaration.

In ES5:

```javascript
var x = {
 a: 1,
 b: 2
}
var a = x.a
var b = x.b
console.log(a) // 1
console.log(b) // 2
```

In ES6:

```javascript
let a, b
({a, b} = {a:1, b:2})
console.log(a) // 1
console.log(b) // 2
```

### Assigning to new variable names

A variable can be extracted from an object and assigned to a variable with a different name than the object property.

In ES5:

```javascript
var o = {p: 42, q: true}
var foo = o.p
var bar = o.q

console.log(foo) // 42
console.log(bar) // true
```

In ES6:

```javascript
let o = {p: 42, q: true}
let {p: foo, q: bar} = o

console.log(foo) // 42
console.log(bar) // true
```

### Default values

A default can be assigned, in the case that the value pulled from the object is undefined.

```javascript
let {a=10, b=5} = {a: 3}  // b isn't reassigned here.

console.log(a) // 3
console.log(b) // 5 (default value returned instead of undefined)
```

### Nested object and array destructuring

Destructuring can be done with nested objects and arrays using a similar syntax:

```javascript
let movie = {
 title: 'blockbuster'
 actors: [{
   firstName: 'Eric Male',
   age: 50
   },
   {firstName: 'Erica Female',
     age: 40
     },
   ]
}

let { title: genre, actors: [,{ firstName: alias }] } = movie

console.log(genre) // blockbuster
console.log(alias) // Erica
```

## Iterative destructuring

`For` loops can iterate through an array of nested arrays/objects and deconstruct them individually.

```javascript
let people = [
  {
    name: 'Mike Smith',
    family: {
      mother: 'Jane Smith',
      father: 'Harry Smith',
      sister: 'Samantha Smith'
    },
    age: 35
  },
  {
    name: 'Tom Jones',
    family: {
      mother: 'Norah Jones',
      father: 'Richard Jones',
      brother: 'Howard Jones'
    },
    age: 25
  }
]

for (let {name: n, family: { father: f } } of people) {
  console.log('Name: ' + n + ', Father: ' + f)
}
// 'Name: Mike Smith, Father: Harry Smith'
// 'Name: Tom Jones, Father: Richard Jones'
```

## Declaring default values in functions

Functions that take in arguments can use assignments to define default values if they are not passed in, within the parentheses that declare which arguments are taken in. These use the `=` assignment operator to declare the default value.

In ES5:

```javascript
const myFunc = function(input) {
  if (input === undefined) input = 'default value'
  console.log(input)
}
```

In ES6:

```javascript
const myFunc = function(input = 'default value') {
  console.log(input)
}
```

These `=` declarations can be nested for functions that take objects as an input:

```javascript
const myFunc = function({property1 = 'default', property2 = 123, property3 = false} = {}) {
  console.log(property1, property2, property3)
}
// if no arguments are passed, default to an empty object
// if an object with no relevant properties are passed, create properties and default to defined values

myFunc() // 'default', 123, false
myFunc({}) // 'default', 123, false
myFunc({property1: 'not default'}) // 'not default', 123, false
```

## Pulling data from objects passed as function parameters

Functions that take in an object as an argument can be immediately deconstructed into variables by putting the deconstruction into the arguments declaration in the function.

```javascript
let user = {
  displayName: 'jdoe',
  fullName: {
      firstName: 'John',
      lastName: 'Doe'
  }
}

function whois({displayName: displayName, fullName: {firstName: name}}){
  console.log(displayName + ' is ' + name)
}

whois(user) // 'jdoe is John'
```

## Computed object property names

If the object property is stored in another variable, wrapping the variable name in `[]` will search for the variable's value instead of the variable name.

```javascript
let myVar = 'key'
let {[myVar]:a} = {key: 1} // [myVar] 'returns' key
console.log(a) // 1
```
