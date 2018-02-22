# ES6 New Math/String/Array/Object APIs
This is part of a series of guides written by students from WDI7 Singapore. Originally written by Wan Shan, Glen and Esther.

## What's new for integers and numbers?

### Overview
In essence, ES6 has ensured greater precision and accuracy for handling integers and arithmetic operations, by adding several new methods that deals with how decimals and floating points of integers are being handled in Javascript.

### 1) Ensuring better accuracy through adding tolerance for rounding errors using Number.EPSILON
This means the smallest interval between two representable numbers. This enables comparison for floating point numbers with tolerance for rounding errors. For example:

```js
0.1 + 0.2 === 0.3 //returns false

function epsEqu(x, y) {
  return Math.abs(x - y) < Number.EPSILON;
}
console.log(epsEqu(0.1+0.2, 0,3));  // returns true
```

### 2)Ensuring better accuracy of large integers using number safety checking methods.
Javascript can only accurately represent within the range of  (−2^53, 2^53). The introduction of these three properties helps to check whether the integers are safe or not safe (i.e., accurately represented in Javascript or not).
  a) Number.isSafeInteger(number)
  b) Number.MIN_SAFE_INTEGER
  c) Number.MAX_SAFE_INTEGER

```js
Number.isSafeInteger(42) === true
Number.isSafeInteger(9007199254740992) === false
```

### 3) Previously global methods that can now be applied to the number prototype.
a) Number.isFinite(number)

Is number an actual number (neither Infinity nor -Infinity nor NaN)?
```js
  Number.isFinite(123)  //  returns true
```

The advantage of this method is that it does not coerce its parameter to number (whereas the global function does):
```js
  Number.isFinite('123') // returns false
  isFinite('123')  // returns true
```
b) Number.isNaN(number)

Is number the value NaN? Making this check via === is hacky. NaN is the only value that is not equal to itself:
```js
let x = NaN;
x === NaN  // returns false
// Therefore, this expression is used to check for it
x !== x  // returns true
```
Using Number.isNaN() is more self-descriptive:
```js
Number.isNaN(x)  // returns true
```
Number.isNan() also has the advantage of not coercing its parameter to number (whereas the global function does):
c) Number.parseFloat and Number.parseInt
The following two methods work exactly like the global functions with the same names.
```js
Number.parseFloat(string)
Number.parseInt(string, radix)
```
## New Math methods?
There are several new Math methods in ES6 that did not exist in ES5. Below is a short explanation:
### 1) Math.sign(x)
Returns the sign of x as -1 or +1. Unless x is either NaN or zero; then x is returned.
```js
Math.sign(-4)  // returns -1
Math.sign(4)   // returns +1
Math.sign(0)  // returns 0
Math.sign(NaN) // returns NaN
```
### 2) Math.trunc(x)
Removes the decimal fraction of x.
```js
Math.trunc(4.5)  // returns 4
Math.trunc(1.2345) // returns 1
```
### 3) Math.cbrt(x)
Returns the cube root of x (∛x).
```js
Math.cbrt(8)  // returns 2
```
### 4) Trigonometric functions
  a)  Math.sinh(x)
      Computes the hyperbolic sine of x.
  b)  Math.cosh(x)
      Computes the hyperbolic cosine of x.
  c)  Math.tanh(x)
      Computes the hyperbolic tangent of x.
  d)  Math.asinh(x)
      Computes the inverse hyperbolic sine of x.
  e)  Math.acosh(x)
      Computes the inverse hyperbolic cosine of x.
  f)  Math.atanh(x)
      Computes the inverse hyperbolic tangent of x.
  g)   Math.hypot(...values)
      Computes the square root of the sum of squares of its arguments.

## Notable String and Array APIs in ES6

### 1) New String APIs - String.prototype.startsWith(), String.prototype.endsWith(), String.prototype.includes()

Three new methods check whether a string exists within another string.

**How do we use it?**

```js
'hello'.startsWith('hell')
//true
'hello'.endsWith('ello')
//true
'hello'.includes('ell')
//true
```

Each of these methods has a position as an optional second parameter, which specifies where the string to be searched starts or ends:

```js
'hello'.startsWith('ello', 1)
//true
'hello'.endsWith('hell', 4)
//true
'hello'.includes('ell', 1)
//true
'hello'.includes('ell', 2)
//false
```
**What problem does it solve?**

Useful when looking for words or expressions.

ES5:

```js
'hello'.indexOf('ello') === 1;
//true
'hello'.indexOf('hell') === (4 - 'hell'.length);
//true
'hello'.indexOf('ell') !== -1;
//true
'hello'.indexOf('ell', 1) !== -1;
//true
'hello'.indexOf('ell', 2) !== -1;
//false
```

### 2) New String API: String.prototype.repeat()

The repeat() method repeats strings.
Only allows for integers and non negative numbers
3.5 will be converted to 3

**How do we use it?**

```js
'doo '.repeat(3)
//'doo doo doo '
```

**What problem does it solve?**

Instead of writing a long code, you can now just write repeat(noOfCount).

ES5:

```js
Array((4 * depth) + 1).join(" ");
Array(3 + 1).join("foo");
```

### 3) New Array APIs: The Array.from()

This new method creates a new Array instance from an array-like or iterable object.

**How do we use it?**
```js
Array.from("foo");
// ["f", "o", "o"]
```
**What problem does it solve?**

Loop will not be required.
Able to create new array from objects that contains arrays.

ES5:

```js
function cast () {
  return Array.prototype.slice.call(arguments)
}
cast('a', 'b')
// <- ['a', 'b']
```

### 4) New Array APIs: Array.prototype.fill()

Convenient utility method to fill all places in an Array with the provided value. Note that array holes will be filled as well.

**How do we use it?**

```js
['a', 'b', 'c'].fill(0)
// <- [0, 0, 0]
new Array(3).fill(0)
// <- [0, 0, 0]
```

You could also determine a start index and an end index in the second and third parameters respectively.

```js
['a', 'b', 'c',,,].fill(0, 2)
// <- ['a', 'b', 0, 0, 0]
new Array(5).fill(0, 0, 3)
// <- [0, 0, 0, undefined x 2]
```

**What problem does it solve?**

Easily fill items in array without having to loop through or find their position

### 5) New Array APIs: Array.prototype.find()

The find() method returns a value of the first element in the array that satisfies the provided testing function.

**How do we use it?**

```js
[ 1, 3, 4, 2 ].find(x => x > 3)
// 4
```
**What problem does it solve?**

Easily fill items in array without having to loop through or create a function.

ES5:

```js
[ 1, 3, 4, 2 ].filter(function (x) { return x > 3; })[0];
// 4
```

## Object API ES5 vs ES6

#### What is the feature??

## object.assign()

object project assignment supports both strings and symbols as keys.For merging objects or cloning them.
New in es6 = { x, y } is an abbreviation for { x: x, y: y }.

## How do we use it?

## Show an example ES5 code vs ES6 code?

## Merging objects

#### ES5

```js
var dst  = { quux: 0 };
var src1 = { foo: 1, bar: 2 };
var src2 = { foo: 3, baz: 4 };
Object.keys(src1).forEach(function(k) {
  dst[k] = src1[k];
});
Object.keys(src2).forEach(function(k) {
  dst[k] = src2[k];
});
dst.quux === 0;
dst.foo  === 3;
dst.bar  === 2;
dst.baz  === 4;
```

#### ES6

```js
var dst  = { quux: 0 }
var src1 = { foo: 1, bar: 2 }
var src2 = { foo: 3, baz: 4 }
Object.assign(dst, src1, src2)
dst.quux === 0
dst.foo  === 3
dst.bar  === 2
dst.baz  === 4
```

## adding method to an object

#### ES5

```js
MyClass.prototype.foo = function (arg1, arg2) {
  ...
};
```

#### ES6

```js
Object.assign(MyClass.prototype, {
  foo(arg1, arg2) {
    ...
  }
});
```

## cloning an object

#### ES5

```js
var copy = Object.assign({ __proto__: obj.__proto__ }, obj);
```

#### ES6

```js
var copy = Object.assign({}, obj);
```

[useful link]http://www.2ality.com/2014/01/object-assign.html

# Acknowledgements

Much of this document has been produced with references to the sites below:

www.2ality.com
http://es6-features.org/
