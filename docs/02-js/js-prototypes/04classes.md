# Classes

Before we examine `class`, let's review. There are a few different ways to create objects.

**Object Literal Notation**

```js
// Object Literal notation uses `var` and `:`
var instructor = {
  name: "Elie",
  age: 26
};

// Or (notice the use of the keyword "new")
var instructor = new Object();

instructor.name = "Elie";
instructor.age = 26;
```

### Problems with Object Literal Notation

Imagine you're an architect and you're tasked with designing a blueprint for a house (which will be used to build 25 similar looking houses). If we assume that every house has a number of square feet, bathrooms and bedrooms we can model this with a few objects.

```js
var house1 = {
  sqFeet: 3000,
  bathrooms: 3
  bedrooms: 2
};
var house2 = {
  sqFeet: 4000,
  bathrooms: 3.5
  bedrooms: 2
};
var house3 = {
  sqFeet: 1000,
  bathrooms: 2
  bedrooms: 1
};
var house4 = {
  sqFeet: 13000,
  bathrooms: 3.5
  bedrooms: 7
};
```

Unfortunately, this is not very efficient. We've created 4 houses and already it's taken almost 20 lines of code. Fortunately we can create a `class` as our "blueprint" and then create objects based off of that.

### Class Literal Notation

We can use a constructor function inside the `class` body to create multiple objects that share the same properties.

Using our previous example, we can create a `class` like this:

```
class House {
  constructor(sqFeet, bathrooms, bedrooms) {
    this.sqFeet = sqFeet;
    this.bathrooms = bathrooms;
    this.bedrooms = bedrooms;
  }  
}
```

Notice our use of the keyword `this`. Since we don't know what the value for the parameters will be, we use `this` as a placeholder. When we call the `House` function, we add in our values. To create an object instance using a constructor function, we use the `new` keyword. Here is an example of how we would create our four houses using a constructor function and the `new` keyword:

```js
var house1 = new House(3000, 3, 2);
var house2 = new House(4000, 3.5, 2);
var house3 = new House(1000, 2, 1);
var house4 = new House(13000, 3.5, 7);
```

## Class Method

As what we've learnt before, the main difference between `class object` and any other `object` is on its ability to own not only property, but also `method`. The term `method` here is a technical terms for functions that the `object` _owned_. This object is also called `instance`.

```
// back to our previous house class

class House {
  constructor(sqFeet, bathrooms, bedrooms) {
    this.sqFeet = sqFeet;
    this.bathrooms = bathrooms;
    this.bedrooms = bedrooms;
  }  

  // we can also attach a method to this class like so

  expansion() {
    // since it's still inside the class body, 'this' here refers to the House 'instance'
    this.bathrooms++
    this.bedrooms += 2
  }

  reduction() {
    this.bathrooms--
    this.bedrooms -= 2
  }
}

```

So attaching `method` is very simple now thanks to `class` notation. No more confusing terms to remember (**prototype** and **constructor**), we just need to remember `class`.

Now that we've attached a function into the `class`, now the function becomes a `method`. The only way for us to call function is through the object `instance`.

```
// this is incorrect way to call class method
// * assumption: class House has been created *
House.expansion() // TypeError: House.expansion is not a function
House.reduction() // TypeError: House.reduction is not a function

// the line above is wrong, because both methods are owned by House INSTANCE

var h1 = new House(200, 2, 2)
h1.expansion()

console.log(h1.bedrooms) // returns 4 because h1 is expanding

```

**Points to note:** `instance` !== `class`
**Try it:** Run the following code in the browser console, and expand each object. Look at the contents. Note that each object has its own version of the `volume` function, even though each copy does the same thing.

```
class Box {
  constructor(length, width, height) {
    this.length = length;
    this.width = width;
    this.height = height;
  }

  volume() {
    console.log(this.length * this.width * this.height);
  }
}

var boxes = [];
for (var i = 1; i <= 100; i++) {
  boxes.push(new Box(i, i, i))
}

console.log(boxes) // array of BOX instances
console.log(boxes[5].volume()) // returns 216 (length: 6, width: 6, height: 6)
```

### Things to watch for

* Using the `new` keyword, Javascript does a few things:
  * Creates a new object
  * Attached the `method` property to the `Class`
* When using the class `method`, don't try to call it without the `new` keyword.
  * Otherwise, you'll get the output of the constructor function, which is `undefined`. The `new` keyword will use the constructor to return a new object.
* Always make sure the keyword `this` is on the left hand side: `this.taco = taco`. Remember, you're assigning parameters to properties of the new object being created.
* `return` statements are usually unnecessary in constructors
