# Classes in ES2015
This is part of a series of guides written by students from WDI7 Singapore. Originally written by Sharona, Ben and Zhao Loong.

## What are Classes?

Classes are 'special functions' and are a simpler way of dealing with the more complex Javascript prototype inheritance models. They are mostly syntactical sugar, ie. It is just syntax which is designed to make things easier to read or to express.

Classes are an inherent part of most other programming languages. This feature allows for easier transition for people migrating from other programming languages to JavaScript.

The inheritance is made simpler using the 'extends' keyword. When a class A extends class B, class A is the child of class B. Using the 'super' keyword, it is possible to inherit the methods defined in the parent class.

Points to be noted:
* A class cannot be hoisted like functions ie. the class cannot be called before first declaring it.
```js
var p = new Polygon();
class Polygon{

}
```

  This leads to a reference error

* Built in objects can also be extended.
```js
class myDate extends Date() {
  constructor() {

  }
  function myExtension() {

  }
}
```

## How do we use Classes?
Syntax wise, classes are similar to constructor functions.

Basic Declaration

```js
class Doggie{
  constructor(name,age){  // syntax to add properties
    this.name = name;
    this.age = age;
  }
  bark(){ // syntax to add a method
    console.log('woof! woof!')
  }
  introduce(){
    console.log('woof! My name is '+ this.name)
  }
}

let lassie = new Doggie('Lassie',3)
lassie.bark()
```

Extends Example

```js
class Pet{
  constructor(){
    this.isHouseTrained = true;
  }
}

class Doggie extends Pet{ //syntax to setup inheritance chain
  constructor(name,age){
    this.name = name;
    this.age = age;
  }
  bark(){
    //some logic
    console.log('woof! woof!')
  }
  introduce(){
    console.log('woof! My name is '+ this.name)
  }
}

var lassie = new Doggie('Lassie',3)
console.log(lassie.isHouseTrained) // true
```

## What problem do Classes solve?
I guess the problem that the es6 syntax solves is that it allows people from different programming backgrounds to easily context switch from different languages ie: python, ruby etc.

in a typical development environment; we changes languages often.
for the front end side we definitely have to use javascript.

things are a little different at the back-end. we are not limited by javascript. Although we still could use javascript with **node**, there are many other frameworks that are available to us. we could use a framework like **ruby on rails** which is built on the ruby language. additionally, we could use something like python with **django**.

outside of javascript, classes and OOP are pretty much defined in a standard way.

thus, the introduction of the class syntax allows these different programmers pick up javascript quickly and write more efficiently w/o the need to learn a language thoroughly(which is frowned upon :( ).

an example of this would be this snippet of python code:
```python
#An example of a class written in Python
#Python demands 4 spaces for indents (what a pain in the ass)
class Shape:
    def __init__(self,x,y):
        self.x = x
        self.y = y
    description = "This shape has not been described yet"
    author = "Nobody has claimed to make this shape yet"
    def area(self):
        return self.x * self.y
    def perimeter(self):
        return 2 * self.x + 2 * self.y
    def scaleSize(self,scale):
        self.x = self.x * scale
    self.y = self.y * scale

```

looking at the es6 class code at the bottom; one can find similarities of the code with the class code written in python.

```js
//this is ES6 code (i have not tested whether it works or not..)
class Shape {
  constructor(x,y) {
      this.x = x;
      this.y = y;
    };
  area() { return this.x * this.y };
  perimeter() { return 2 * this.x + 2 * this.y };
  scaleSize(scale) {
    this.x = this.x * scale;
    this.y = this.y * scale;
  }
}
```

lastly let's look at an example of traditional prototypical style of writing the shape class
```js
//this is ES5 code (might be wrong i'm not an expert in OOP)
function Shape(x,y){
  this.x = x;
  this.y = y;
  this.area = function() { return this.x * this.y };
  this.perimeter = function() { return 2 * this.x + 2 * this.y };
  this.scaleSize = function(scale){
    this.x = this.x * scale;
    this.y = this.y * scale;
  }
}
```
## ES5 code vs ES6
This is the code written in ES5 syntax.
examples taken from : https://medium.freecodecamp.com/learn-es6-the-dope-way-part-v-classes-browser-compatibility-transpiling-es6-code-47f62267661#.7x3fkox7e

```js
function Bunny(name, age, favoriteFood) {
  this.name = name;
  this.age = age;
  this.favoriteFood = favoriteFood;
}

Bunny.prototype.eatFavFood = function () {
  console.log('\"Mmmm! Those ' + this.favoriteFood + ' were delicious\", said ' + this.name + ', the ' + this.age + ' year old bunny.');
};

var newBunny = new Bunny('Brigadier Fluffkins', 3, 'Raspberry Leaves');
newBunny.eatFavFood();
// "Mmmm! Those Raspberry Leaves were delicious", said Brigadier Fluffkins, the 3 year old bunny.
```

This is the code that is refactored into ES6 syntax.

```js
class Bunny {
  constructor(name, age, favoriteFood){
    this.name = name;
    this.age = age;
    this.favoriteFood = favoriteFood;
  }

  eatFavFood() {
    console.log(`"Mmmm! Those ${this.favoriteFood} were delicious", said ${this.name} the ${this.age} year old bunny.`);
  };
}

let es6Bunny = new Bunny('Brigadier Fluffkins', 3, 'Raspberry Leaves');
es6Bunny.eatFavFood();
// "Mmmm! Those Raspberry Leaves were delicious", said Brigadier Fluffkins the 3 year old bunny.
```

## Is this the 80% or 20%?
Looking at the future of javascript; as the language matures further; more people from traditional OOP style of coding would get into javascript.

The only reason why the class syntax is introduced is so as to ease these traditional CS graduates into the language itself.

Therefore, this looks to be the trend to follow; so our team would conclude that this would be the 20% in the future.

## Acknowledgments
Much of the code were inspired by [this post](https://medium.freecodecamp.com/learn-es6-the-dope-way-part-v-classes-browser-compatibility-transpiling-es6-code-47f62267661#.7x3fkox7e). The authors of this guide give credit to that author.
