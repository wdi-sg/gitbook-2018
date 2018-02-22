# OOP with Prototypes

## Objectives

* Identify the downsides of object literal notation.
* Use constructor pattern to create multiple object instances.
* Understand the basic premises behind object-oriented programming with prototypes in JavaScript.
* Use constructors and prototypes to modularize code and keep code DRY.

## Object Oriented Programming

> Object-oriented programming (OOP) is a programming paradigm based on the concept of "objects", which are data structures that contain data, in the form of fields, often known as attributes; and code, in the form of procedures, often known as methods. A distinguishing feature of objects is that an object's procedures can access and often modify the data fields of the object with which they are associated (objects have a notion of "this"). In object-oriented programming, computer programs are designed by making them out of objects that interact with one another. - [wiki](http://en.wikipedia.org/wiki/Object-oriented_programming)

We've been encountering examples of object-oriented programming in JavaScript, but there's ways to make objects easier and more efficient to construct. Specifically, we'll be looking at **class** and **extend** in order to achieve these goals.

Bear in mind, if you google `OOP Javascript`, you'll probably come across **[constructor](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/constructor)** and **[prototypes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/prototype)**. Those are valid javascript methods to create object before, but it's complicated to understand and the global webdev community is moving towards the new version of javascript now, [ES2015](../js-next/01readme.md). 

<!-- We've been encountering examples of object-oriented programming in JavaScript, but there's ways to make objects easier and more efficient to construct. Specifically, we'll be looking at **constructors** and **prototypes** in order to achieve these goals. -->
