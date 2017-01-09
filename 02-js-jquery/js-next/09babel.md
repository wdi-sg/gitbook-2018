# Babel
This is part of a series of guides written by students from WDI7 Singapore. Originally written by Adrian and Yi Sheng.

## What is Babel?

Babel is primarily a transpiler, or source-to-source compiler - a type of compiler that converts source code written in one language and produces the equivalent code in another language. In the case of Babel, its most common usage case is code conversion from ES6 (ECMAScript 6th Edition, officially known as ECMA Script 2015) to ES5 (ECMAScript 5 Edition).

At this point in time (start of 2017), browser support for ES6 is incomplete. Transpiling to ES5 allows code to run more reliably on today's browsers.

Transpiling in practice is an additional step in the build process (here, the process of going from source code to something that will run directly on the browser or other interpreter ). An alternative method to transpiling would be to use a polyfill - a piece of code which when included allows use of additional functionalities. This results in additional functionality being defined at the start of runtime, much like a front-end framework. Babel has a polyfill available.

## How to use Babel as a transpiler

To use Babel transpilation in a Node.js project, use npm to install it locally.
Babel version 6, unlike Babel 5.x, requires the selection of a preset.

```
  npm install --save-dev babel-cli babel-preset-env
```

The above installs the env preset, which automatically determines the Babel plugins you need based on your supported environments.

A .babelrc file is also required in your project; alternatively you can include the following in your package.json.

```
  {
    "presets": ["env"]
  }
```

More detailed installation instructions are available on [the Babel site](http://babeljs.io/docs/setup/#installation)

## How to use Babel as a polyfill

Babel as a transpiler only transforms syntax (arrow functions and so on.) Support for new globals such as Promise and native methods such as String.padStart(left-pad) can also be added by installing the Babel polyfill to a project:

```
npm install --save-dev babel-polyfill
```

The polyfill can be included by requiring it at the top of the entry point to your application, or in your bundler config.

## Using Babel to convert code in browser

You can also try out converting code on the [converter on the Babel site](http://babeljs.io/repl/#?babili=false&evaluate=true&lineWrap=false&presets=latest%2Creact%2Cstage-2&experimental=false&loose=false&spec=false&code=%5B1%2C2%2C3%5D.map(n%20%3D%3E%20n%20%2B%201)%3B&playground=true)

## Some examples of Babel conversions

### Arrows

ES6
```js
nums.forEach(v => {
  if (v % 5 === 0)
  fives.push(v);
})
```

ES5
```js
nums.forEach(function (v) {
  if (v % 5 === 0) fives.push(v);
});
```

### Let + Const

ES6
```js
function f() {
  {
    let x;
    {
      // this is ok since it's a block scoped name
      const x = "sneaky";
    }
    // this is ok since it was declared with `let`
    x = "bar";
  }
}
```

ES5
```js
function f() {
  {
    var x = void 0;
    {
      // this is ok since it's a block scoped name
      var _x = "sneaky";
    }
    // this is ok since it was declared with `let`
    x = "bar";
  }
}
```

### Default Parameter Value
ES6
```js
function f (x, y = 7, z = 42) {
  return x + y + z
}
f(1) === 50
```

ES5
```js
function f (x, y, z) {
  if (y === undefined)
  y = 7;
  if (z === undefined)
  z = 42;
  return x + y + z;
};
f(1) === 50;
```

### Template String
ES6
```js
var customer = { name: "Foo" }
var card = { amount: 7, product: "Bar", unitprice: 42 }
var message = `Hello ${customer.name},
want to buy ${card.amount} ${card.product} for
a total of ${card.amount * card.unitprice} bucks?`
```

ES5
```js
var customer = { name: "Foo" };
var card = { amount: 7, product: "Bar", unitprice: 42 };
var message = "Hello " + customer.name + ",\n" +
"want to buy " + card.amount + " " + card.product + " for\n" +
"a total of " + (card.amount * card.unitprice) + " bucks?";
```
