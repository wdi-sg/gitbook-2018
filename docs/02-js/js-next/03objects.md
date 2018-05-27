# Enhanced Object Literals
This is part of a series of guides written by students from WDI7 Singapore. Originally written by Andrew, Cheng Koon and Melvin.

## What is the feature?

Recall:

Object literal = list of property names and associated values of an object, enclosed in curly braces.

There are 3 features:

1. Shorthand syntax for initializing properties from variables.
2. You can have computed property keys in an object literal definition.
3. Shorthand syntax for defining function methods.

## How do we use it?

```js
function getBike (make, model, value) {
  return {
    // Property value shorthand syntax
    // You can omit property value if key matches variable
    // JavaScript engine looks in containing scope for variable with same name
    // If found, that variable's value is assigned to the property
    // If not, ReferenceError thrown

    make, // same as make: make
    model, // same as model: model
    value, // same as value: value

    // Computed values now work with object literals
    ['make' + make]: true,

    _value: value,

    get value() {
      return this._value
    },
    set value() {
      if (value < 0)
        throw new Error('Invalid value')
      this._value = value
    }

    // Method definition shorthand syntax
    // 'Function' keyword and colon now omitted
    depreciate() {
      this.value -= 1500
    }
  }
}

let bike = getBike ('Honda', 'TW-200', 10000)

console.log(bike)
// output: {
//   make: 'Honda'
//   model: 'TW-200'
//   value: 10000
//   makeHonda: true
//   depreciate: function()
// }

bike.depreciate()

console.log(bike.value)
// output: 8500

console.log(bike.value = 9000)
console.log(bike.value)
// output: 9000
```

## What problem does it solve?

ES6 makes your syntax in declaring object literals more succinct.

## Example ES5 v ES6 code

ES5:
```js
function getBike (make, model, value) {
  var bike = {}
  bike['make' + make]: true

  return {
    make: make,
    model: model,
    value: value,

    bike,

    depreciate: function () {
      this.value -= 1500
    }
  }
}
```
ES6:
```js
function getBike (make, model, value) {
  return {
    make,
    model,
    value,

    ['make' + make]: true,

    depreciate() {
      this.value -= 1500
    }
  }
}
```
## How useful is this feature to us, really?

I think we've realized that ES6 is becoming increasingly popular and it's really good to get into it as soon as possible. Even if you're worried about browser compatiblity, [Babel](https://babeljs.io/) will help.

## Links for your reference

* **Ben Ilegbodu** - *Author* - [Learning ES6: Enhanced Object literals](http://www.benmvp.com/learning-es6-enhanced-object-literals/)

* **Cory Ryan** - *Author* - [JavaScript ES6 Class Syntax](https://coryrylan.com/blog/javascript-es6-class-syntax)
