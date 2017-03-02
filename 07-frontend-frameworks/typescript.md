# TypeScript

## Objectives

- Describe what transpiling is and why we might want to use it
- Set up the TypeScript compiler and use it to transpile our code to JavaScript
- Divide our code into multiple modules and use SystemJS or WebPack to load them - understanding the difference of each approach.

## What is TypeScript and why use it?
TypeScript is a Superset of Javascript. What that means is that it is still fundamentally JavaScript, but it adds additional features. The browser however, does not understand TypeScript, so we need to transpile it back to JavaScript.

> **Transpiling** is the process of converting a language to another language that is of a similar level of abstraction (different from compiling, where say converting our c++ code into executable machine code). You may already have encountered Babel, as a tool fro transpiling ES6 into ES5, or even in Rails we transpile from CoffeeScript to JavaScript, or Sass to CSS.

This means that we can write in TypeScript (and benefit from it's additional features) but still end up with plain JavaScript when it renders in our user's browser.

TypeScript adds two main benefits to our JavaScript.

### ES6 & ES7 Support
TypeScript implements the ECMAScript standard and so allows us to use the ES6 features we've learned in addition to some experimental ES7 features like Decorators - more on that later.

### Static Typing
In JavaScript we're used to Dynamic/Duck Typing - if it looks like a duck it is a duck. This means that all of our checking is at runtime. If for example, an object happens to have a function called `quack` by the time we try to access it then we are happy, but if that funtion is not defined then we throw an error. Either way we don't find out until run-time. This gives us a lot of flexibility but also makes it hard to create reliable applications without huge amounts of tests.

In Static Typing, we are explict when saying what types of Objects we assign to variables and properties. We also say what types our functions return. If we try to use it in an incorrect way, we get an error before at 'compile/transpile time' e.g. before we even run our application. TypeScript adds this functionality to JavaScript allowing us to detect and avoid errors earlier.

## Getting Started
You can [Try TypeScript online](https://www.typescriptlang.org/play/index.html), but it is super easy to install locally using npm.


```bash
npm install -g typescript
```

To get the best out of TypeScript you need your editor to support it. You can either install a package for TypeScript Linting or try out Microsoft's Free Code Editor [Visual Studio Code](https://code.visualstudio.com).

The next step is to make a new project directory and npm init.

```bash
mkdir typescript101 && cd typescript101
touch index.ts
npm init -y
```

Open up the `index.ts` file and add a normal JavaScript console log to it.

```js
console.log('Hello TypeScritpt')
```

Now to compile this, we just run the TypeScript compiler (tsc).

```
tsc index.ts
```

A .js file will now be created in the same directory. Let's run it in Node.

```
node index.js
```

We should see our console log. That's it's we've written our first TypeScritpt project and transpiled it to JavaScript. Ok there wasn't much that changed but we'll get there. First let's improve our build process.

### TSConfig
For a larger project, we'll want to config TypeScript across our project and have it compile all files in our directory. The easiest way to do that is to create a `tsconfig.json` file. Inside here we can configure an number of options include the source and destination directories. For now we'll specify: what version of JS we want the code transpiled into, let it know that we're using experimental decorators, and finally tell it we want sourceMaps (a way for the browser to link the JS code to the TS that generated it).

**tsconfig.json**

```json
{
  "compilerOptions": {
    "target": "es6",
    "experimentalDecorators": true,
    "sourceMap": true
  }
}
```

If we run TypeScript with the -w option, it will now watch our files and automatically compile them if they change.

```
tsc -w
```

### Basic Build Process with NPM

We know that we can use TypeScript to watch for .ts file changes and we can use Nodemon to relaunch node if a .js file changes, so let's combine the two.

We can use NPM Scripts, to specify a start script that: compiles code, then runs typescript in watch mode whilst concurrently running nodemon.

> _This makes use of && to run commands in sequence and & to run commands concurrently, this may be different on Windows, in which case you can check out the concurrently package on npm._

```json
"scripts": {
    "start": "tsc && (tsc -w & nodemon index.js)"
  }
```

Now anytime we create or modify a .ts file it will be converted into a .js and Nodemon will restart. So let's start writing some TypeScript.

## TypeScript Fundamentals
Let's look at what we can do in TypeScript.

For starters we can do pretty much all the ES6 stuff we're used to, without worrying about browser compatibility:
- default function params
- template strings
- let/const
- for of
- fat arrow functions
- deconstructing
- spread operator

We'll assume you know all that and move onto what TypeScript adds to the mix, namely Static Typing.

### Essential Types
Types for variables or params are typically specified using a colon followed by the name of the type. We have the following types available to us in TypeScript
- boolean `:boolean`
- number `:number)`
- string `:string`
- any `:any`
 - *consider this the default js type ie. anything is acceptable)*
- void `:void`
 - uses as return type for function to specify no return value
- array of type (two synatax options: `:number[]` or `:Array<number>`)

[Checkout the Full List online](https://www.typescriptlang.org/docs/handbook/basic-types.html)

For Objects, Classes and Functions we'll can use the Interface construct described later.

### External Types
When we include external JavaScript libraries, we ideally want TypeScript to know about the types for that library. This used to be done using a tool called tsd, then one called typings, the easiest way at the moment (as of TypeScript v2) is to just use npm.

When we install a package we can then use npm to install the typings for that package using an @types syntax, for example

```bash
    npm install --save jquery
    npm install --save @types/jquery
```

This will then give us the Types in our project and allow us better code hinting for that library.


### Variable Types
When we declare a variable we can TypeScript what type that variable will contain, e.g. a number:
```ts
let age : number
```

If we try to assign a value to that which is not of that type we'll then get an error.

```ts
let age : number

age = 25
age = "25" // error TS2322: Type '"25"' is not assignable to type 'number'.
```

If we don't specify a type, then TypeScript will assign one from the initial assignment (clever TypeScript).

```ts
let age = 25
age = 35
age = "25" // error TS2322: Type '"25"' is not assignable to type 'number'.
```

### Types in Functions
With TypeScript we can specify what type a function parameter should be. If we pass an incorrect type, we'll then get an error.

```ts
function greetUser(user:string) {
    console.log(`hello ${user}`)
}

greetUser('Jeremiah') // ok
greetUser(5) // error TS2345: Argument of type '5' is not assignable to parameter of type 'string'
```

We can also specify what type of value a function should return, by adding the type before the curly braces.

```ts
function capitalize(word:string):string {
    return word[0].toUpperCase() + word.slice(1)
}

let capitalizedName : string = capitalize('jeremiah') // ok
let age : number = capitalize('jeremiah') // error TS2322: Type 'string' is not assignable to type 'number'.
```

#### Union & Overloads
If we want some flexibility in our function params, we can specify a union (an OR option for our function). In the example below, we allow a string or an array and then use the instanceof operator as a 'guard' to tell which one it is.

```ts
function unionExample( x:(string | any[]) ):void {
    var len = x.length

    if (x instanceof Array){
        console.log(`array is length: ${len}`)
    } else {
        console.log(`string is length: ${len}`)
    }
}
```

### Interfaces / Custom Types
When working with Objects, we can specify our own types using Interfaces. An interface looks like a lot like an class except, we never instantiate an interface.

We can make a Key Value Pair in our interface optional by specifying a question mark after the name.

```ts
interface Task {
    name:string
    completed?:boolean
}
var task1 : Task = { completed: false } // error TS2322: Type '{ completed: false; }' is not assignable to type 'Task'. Property 'name' is missing in type '{ completed: false; }'.

var task2 : Task = { name: 'buy milk' } // ok
```

#### Classes & Implementing Interfaces
Whilst we cannot instantiate a interface, we can create a class that implements the given interface. Effectively, we use interfaces as a wat of defining the methods that a class must have. So, if we then say that our class implements a specific interface, TypeScript will tell us if we are missing any methods.

```ts
interface IPrinter {
    print():string
    printToConsole():void
}

class DotPrinter implements IPrinter {
    message:string = "hello world"
    print(){
        return this.message
    }
} // error TS2420: Class 'DotPrinter' incorrectly implements interface 'IPrinter'. Property 'printToConsole' is missing in type 'DotPrinter'.
```

#### Function Interfaces
Functions can also use interfaces, we just define a function property without a name

```ts
interface OperatorInterface {
    (x:number, y:number):number
}

var add:OperatorInterface  = function (x:number, y:string):number {
    return x + y
} // error TS2322: Type '(x: number, y: string) => number' is not assignable to type 'OperatorInterface'.
```

### Enums
TypeScript also introduces Enums. You can think of enums as a custom value type, i.e. a way of creating a group of valid values which a variable can be, but it is not allowed to be any other. This is super usful for representing options or states, as illustrated below.

```ts
enum OrderState {
    New = 1,
    Active,
    Complete,
    Deleted
}

let orderStatus:OrderState = OrderState.New
// ...
orderStatus = OrderState.Complete
// ...
if ( orderStatus == OrderState.Complete) {
    console.log('order is complete');
}

```

The above us much easier to read and debug that the equivilent with numbers or strings.

### Access Modifiers: Public, Private, Protected
When we create classes in TypeScritpt, as well as defining types, we can specify how the properties can be accessed, namely: public, private or protected.
- **Private** means that the property can only be accessed by the methods in this class
- **Protected** is the same as private except it allows any subclasses to also access the property in its methods
- **Public** is the default and means the property can be directly accessed on any instance

```ts
class BankAccount {
    protected type:string;
    private balance:number;
    public name:string;
    constructor(){
        this.name = 'default'
        this.type = 'current'
        this.balance = 0
    }

    // Methods can also have access modifiers
    private empty(){
        this.balance = 0
    }
}
let jeremiahsAC = new BankAccount()
jeremiahsAC.name = 'JEREMIAH' //ok
jeremiahsAC.type = 'savings' //error
jeremiahsAC.balance = 100 //error

class SavingsAccount extends BankAccount {
    constructor(){
        super()
        this.name = 'default' //ok
        this.type = 'savings' //ok
        this.balance = 0 //error
    }
}
```

### Generics
One of the more advanced features in TypeScript are Generics. Generics allow us to create functions and classes that can support various types. When you set one up, you effectively alias the type so that you can reference it, throughout the rest of the definition. The example below is a generic clone function, it takes in type T and and returns type T, what T is, is defined at the point the Generic is initalise.

```ts
function clone<T>(value:T):T{
    return JSON.parse( JSON.stringify(value) )
}

var name:string = clone<string>('jeremiah') //ok
var age:number = clone<number>(5) //ok
var age:number = clone<number>("5") //error - Argument of type '"5"' is not assignable to parameter of type 'number'.
```

Generics can get pretty complex, so you won't use them often but they're are a powerful technique for creating versatile code whilst still retaining type checking.

### Intro to Decorators

**NOTE** *Decorators are an experimental JavaScript feature that may change in future releases. To enable experimental support for decorators, you must enable the experimentalDecorators compiler option in your tsconfig.json.*

Decorators are a mechanism for us to anotate classes, their properties and methods at run time - this is Meta-Programming, where we alter the behaviour of our classes at run-time. This is a tough concept and difficult to understand, so let's first look at how we might implement a Decorator pattern with vanilla JavaScript.

In the following example we have a function called sayHello, we decorate this function by calling it from our logDecorator function, all our logDecorator does it simply output the name of the function it is calling.

```js
function sayHello(){
  console.log('hello')
}

function logDecorator(target){
  console.log(`calling the ${target.name} function on ${this}`)
  target()
}

logDecorator(sayHello)
```

In TypeScript, we would achieve the same using the @decorator notation, with which we no longer need to call the function directly.

```ts
class Greeter {
    @logDecorator
    sayHello(){
        console.log('hello')
    }
}

// our decorator needs to match a specific function pattern as defined by TypeScript.
function logDecorator(target:any, propertyKey:string, descriptor:PropertyDescriptor):void {
  console.log(`calling the ${propertyKey} function on ${target}`)
}


let greeter = new Greeter()
greeter.sayHello()
```

#### Class Decorators
When used with a class, our decorate receives the constructor function. In the example below, we use a class decorator to modify the prototype of the Hollera class so it extends the Greeter class.

```ts
class Greeter {
    sayHello(){
        console.log('hello')
    }
}

@changePrototypeDecorator
class Hollera {
  constructor() {
    console.log('Hollera created')
  }
}

function changePrototypeDecorator(constructor: Function) {
    let obj = Object.create(constructor.prototype, {})
    Object.assign(obj, Greeter.prototype)
    console.log(`modified protoype`)
}

let hollera = new Hollera()
hollera.sayHello()
```

#### Property Decorators
Decorator on properties are also possible but require the use of an experimental api called reflect-metadata to be used, and are beyond the scope of this introduction.

#### Decorator Factories
We've seen how decorators work but they could be more even more powerful, if we could pass them additional information, this can be achieved using a decorator factory.

Our decorators must match a specific parameter list, so if we create a function that returns a function that matches that requirement, then we can store state using closure. Stay with me, let's look at a vanilla JS example.

In the following example we have a function to greet, we then create a decoratorFactory, which returns a decorator that calls the method but binds the given person to the this value, using the call method.

```js
function greet(){
  console.log(`${this.name} says hello`)
}

function decoratorFactory(person){
  return function decorator(funcToDecorate) {
    funcToDecorate.call(person)
  }
}

var jeremiahDoDecorator = decoratorFactory({name: 'jeremiah'})
jeremiahDoDecorator(greet)
```

Let's explore how something similar to this might work in TypeScript. In the following example, we decorate a class by inserting a person into the prototype chain.

```ts
let jeremiahsDetails = {name: 'jeremiah'}

@decoratorFactory(jeremiahsDetails)
class Greeter {
  greet(){
    console.log(`Hi, my name is ${this.name}`)
  }
}

function decoratorFactory(person: string) {
  return function (constructor: Function) {
    let obj = Object.create(constructor.prototype, {})
    Object.assign(obj, person)
    constructor.prototype = obj
  }
}

let jeremiah = new Greeter()
jeremiah.greet()
```

Decorators can be pretty complex, so don't worry if it's not super clear. However, it is useful to understand how they work because they're used heavily in Angular2 and contribute to it's incredibly readability.

## TypeScript in the Browser
So far we've seen how we can use TypeScript to work with Node but what about the Browser? If we have only 1 JS file then we can still just link to the JS file as normal in our html. Then instead of Nodemon, we can use a browser-syncing package like lite-server to serve our page and auto-refresh whenever a file changes.

```bash
npm install -g lite-server
```

Our start script can then run lite-server instead and anytime a file changes our browser will refresh.

```json
"scripts": {
    "start": "tsc && (tsc -w & lite-server index.js)"
}
```

Notice that in the browser we can click on console output and errors and magically see the TypeScript that generted it, this is thanks to the source maps generated by TypeScript.

## Module Management
Working with ES6 we can seperate our code into modules and use imports and exports to link them together. This is much like what we do in Node except we can use the ES6 module sytax.

> **Internal/External:** this is called external module mangement. TypeScript also supports internal modules and mixins using Namespaces, like we've seen in Ruby.

**my-class.ts**

```ts
export class MyClass {
    constructor() {
        console.log('MyClass created')
    }
}
```

**index.ts**

```ts
import {MyClass} from './my-class'
```

Just like much of ES6, this is not currently not supported in browsers, so we need to use an external library to manage out modules. The first step is to tell TypeScript what module system we plan on using, so it can transpile into the correct format.

### In Node - CommonJS
The module loading standard in Node is called CommonJS (e.g. `require("./myModule")`). If we're working with Node then we should configure our tsconfig to export in that style - nice and easy.

```json
{
  "compilerOptions": {
    "target": "es6",
    "module": "commonjs",
    "experimentalDecorators": true,
    "sourceMap": true
  }
}
```

### In Browser - SystemJS
If we're working in the browser and want to have multiple files. then we'll need to use a Module Loader like [SystemJS](https://github.com/systemjs/systemjs) or a bundler like WebPack. SystemJS is quick to get started with and is fairly easy to use with TypeScript so let's look at that first. SystemJS, does what is called lazy-loading and so only loads the files as they are imported.

We can download/npm the SystemJS library to reference it locally or include the CDN link in our html. We then configure it to treat a particular file as our app entry point and to treat all other files from there as using a default .js extension. Notice in our example, that our file is not actually in an app folder, so we map 'app' to the site root.

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/systemjs/0.20.9/system.js"></script>
<script>
    System.config({
        map: { app: '/' },
        packages: { 'app': { base: './', main: 'index.js', defaultExtension: 'js' } }
    })
    SystemJS.import('app').then(function(module) {
        console.log('Module Loaded')
    });
</script>
```

### In Browser - WebPack
Webpack bundles all of our JS files into one (much like Rails) and loads that file for us. Slower initial load but quicker afterwards. If we want to use Webpack, we first we need to install it.

```bash
npm install -g webpack
```

Once we use Webpack, we no longer use the TypeScript compiler directly, instead we use a webpack loader (plugin) to do it for us. Let's install both in our project, so we have them as dependencies.

```bash
npm install --save-dev typescript awesome-typescript-loader
```

Next we need to create a configuration file for WebPack. There are loads of options that we can put in there but the example below is enough to get us started. We specify the entry point of our application, a name and location for output bundle, as well as telling it what extensions to support and what loader to use to to process them.

**webpack.config.js**
```js
module.exports = {
    entry: "./src/index.ts",
    output: {
        filename: "bundle.js",
        path: __dirname + "/dist"
    },
    // Enable sourcemaps for debugging webpack's output.
    devtool: "source-map",
    resolve: {
        // Add '.ts' and '.tsx' as resolvable extensions.
        extensions: [".ts", ".tsx", ".js"]
    },
    module: {
        loaders: [
            // All files with a '.ts' or '.tsx' extension will be handled by 'awesome-typescript-loader'.
            { test: /\.tsx?$/, loader: "awesome-typescript-loader" }
        ]
    }
};
```

Let's config TypeScript to use commonjs and whilst we're here, let's also tidy up our folder structure. By introducing a 'source' folder and also a 'distribution' folder. We'll keep our .ts files in the source and our .js files and index.html in the distribution folder, ready to be deployed to a server.

```json
{
    "compilerOptions": {
        "outDir": "./dist/",
        "sourceMap": true,
        "experimentalDecorators": true,
        "module": "commonjs",
        "target": "es5"
    },
    "include": [
        "./src/**/*"
    ]
}
```

In our html, we can now just include our bundle.js script.

```html
<body>
    <script src="bundle.js"></script>
</body>
```

If we now run `webpack` it will create the bundle.js file for us. If we open our `index.html` file in the browser we can see the results. However, Webpack also has it's own development server that we can use, to have live-reloading, so let's insatll it:

```bash
npm install -g webpack-dev-server
```

We can then run the dev server, telling it to look inside our distribution folder for the content.

```bash
webpack-dev-server --content-base dist/
```

We can save this in our npm scripts for convenience.

> *Important Note*. Webpack Dev Server, doesn't create the bundle.js file instead it serves the results in real-time, so to deploy we'll still need to run webpack, we can add this as an npm build task.

```json
"scripts": {
    "start": "webpack-dev-server --content-base dist/",
    "build": "webpack"
  }
```


## Conclusion

TypeScript is a powerful addition to JavaScript and something you can consider using for larger projects. Also knowing TypeScript opens the doors for you to now learn Angular2, a very powerful front-end JavaScript framework.
