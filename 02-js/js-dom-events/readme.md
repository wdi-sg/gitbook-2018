#Intro to DOM and Events

---

## Objectives
* Select elements from the DOM using selectors
* Add events to elements in the DOM

---

##DOM Manipulation with JavaScript

**Review:** What is the DOM?

Open up the [MDN Website](https://developer.mozilla.org/en-US/)

Go to Developer Console. Look at DOM in *Elements*, then look at the DOM in *Console*. The object 'document' represents the DOM in JavaScript. We can change the DOM, i.e. the page, by changing the **document object**.

Review [DOM on MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)

---

Now, inspect a few properties, for example:

```js
document.URL
document.head
document.links (what does it return?)
```

---

How to change the DOM? Select elements and manipulate them.

**Select by tag id:**

```js
var searchForm = document.getElementById("home-search-form");

```

What's inside?

```js
searchForm.innerHTML;
```

Also, try `textContent` too! (you may also see this as `innerText`, but this is not supported by all browsers)

---

Change styling:

```js
searchForm.style.backgroundColor = "yellow";
searchForm.style.color = "red";
searchForm.style.height = "100px";
```

---

Properties can be a getter and setter. What does this mean?

**Select by class**

```js
var content_div = document.getElementsByClassName("center");
```

**Select by tag name**
```js
var allListElements = document.getElementsByTagName("li")
```

---

**Preferred: select using CSS selectors**

Get elements by tag name or class is very unspecific. You can go after specific CSS selectors, just like you would in stylesheets:

```js
document.querySelectorAll("li");
document.querySelectorAll("li.nav-main-item");
document.querySelectorAll(".center > h1");
```

---

**Accessing and changing element attributes**

There are 2 ways to get and set attributes of a DOM element. You can access the properties directly or use use get/setAttribute methods. It's important that you know both exist, but generally accessing the properties directly is more consistent across browsers.

```js
//set using property
document.querySelector("a").href = ' http://www.google.com';

//get using property
var href = document.querySelector("a").href;

//get using getAttribute method
var href = document.querySelector("a").getAttribute("href");

//set using setAttribute method
document.querySelector("a").setAttribute("href","http://www.google.com")
```
---

##CSS Classes

Acessing, getting, setting CSS classes is slightly different than other properties.

---

##className

First you can directly access the class attribute by using the `className` property of a DOM element. This works fine, but since elements can have multiple classes (separated by spaces) this often leads to needing to do some string parsing.

---

##classList

To solve the problem of not wanting to do string parsing of the `className` property browsers support the `classList` attribute which gives us an array of classes.

Additionally, the classList attribute has some special methods attached to it.

* add - add a class
* remove - removes a class
* contains - checks if an item has a class

---

**Usage**

```js
//list classes
item.classList

//add a class
item.classList.add('my-new-class');

//remove a class
item.classList.remove('my-new-class');

//check if an item has a class (returns true or false)
item.classList.contains('my-new-class');
```
---

## Compare and Contrast

Compare and contrast the following selectors. Why can't we use querySelector/querySelectorAll for everything?

* getElementById
* getElementsByClassName
* getElementsByTagName
* querySelector
* querySelectorAll

---

### Pairing Exercise (15 minutes)

* Open up your browser and go to the MDN Website.
* Repeat all of the exercies up to this point in the console.

---

##[Events](#events)

* Events are a fundamentally different way to think about the design of our programs.

* Order of execution is now non-linear- It depends upon the actions of the user.

[async programming can be hard](https://twitter.com/julia_asterisk/status/829045121933000708)

Now that we know how to select DOM elements, we can **attach** events to them:

- Common Events:
	- change
	- click
	- mouseover
	- mouseout
	- keydown
	- keyup

* [List of Event Types](https://developer.mozilla.org/en-US/docs/Web/Events)

---

## How Events Work

* User does some action ( or something happens in the browser that a listener is set to )
* Browser runs the function associated with that event
* Just like `setTimeout` and `setInterval`, we specify something to be run at a later time.

---

#####addEventListener

We have different **events**, but now we need a way to attach these events to elements. The solution? Use the `addEventListener` function. This function is called on DOM elements and takes two parameters: an **event type** and a **function**. The event type refers to a "click", "mouseover", or other type of event. The function contains the code that runs when the event occurs.

Also this is the style you should use often.

```js
document.getElementById("myDiv").addEventListener("click", function() {
	//Your code here
}
```

---

Events can only be attached to specific elements. Therefore, when you return a collection such as the result of `document.querySelectorAll` you **CANNOT** simply do this:

```js
document.querySelectorAll(".li").addEventListener("click", function() {
	console.log("Click worked");
}
```

---

Instead you must loop through all of the elements and attach an event to each item individually.

```js
var listItems = document.querySelectorAll(".li");
for(var i = 0; i < listItems.length; i++){
    listItems[i].addEventListener("click", function() {
    	console.log("Click worked");
    }
}
```

---

## Note: Method Chaining

* For DOM methods and events we can use the method chaining syntax so that we don't have to capture return values and call methods on those assigned values.

Without method chaining:
```
var element = document.getElementById("myDiv");

element.addEventListener("click", function() {
	//Your code here
});
```

With method chaining
```
document.getElementById("myDiv").addEventListener("click", function() {
	//Your code here
});
```

Another syntax
```
document.getElementById("myDiv")
        .addEventListener("click", function() {
          //Your code here
        });
        // be sure to indent the dot method to the previous one
```
---

### DOMContentLoaded

All of the selectors we've been using rely on the use of DOM elements. However,
if the JavaScript loads before all the DOM elements load, the selectors won't
recognize that some of them exist! To avoid this problem, there's an event
called `DOMContentLoaded` that we can encapsulate our code inside. Then, we
can guarantee that the DOM elements exist before manipulating them.

```js
document.addEventListener('DOMContentLoaded', function() {
  //code and events go here
});
```

---

### Pairing Exercise: (15 minutes)
Paste this function into your console:
```
var saySomething = function(){
  alert("YES!");
};
```

Set this function as a callback to some events.

* set the click event to the main h1 text `.hightlight-span`
* set mouseover to that element
* set keyup or keydown to the search box

---

# Connecting events to the DOM
## How do we refer to an element that was clicked on?
#### What program flow would we use to recreate the tictactoe game?

---
With the tools we have now we can do this:

```
document.getElementById("user-input");
        .addEventListener('change',function(){
          // we have a click event

          // re-get the element that was clicked on
          var input = document.getElementById('user-input');
        });
```

But we can do better and not repeat ourselves.

---
### Method 1: `event` parameter
- the browser, when executing your callback function, will pass in the event object parameter.
- A dom event object. Contains everything about the event that occured:
- where it occured from
- pixel coordinates for the event
- etc., depending on the type of event
- the DOM element that originated the event will be set to `event.target`



---
### Example 1: User Text Input

If you have this HTML:
```
<input id="user-input"/>
```

You can write this in your javascript:
```
document.getElementById("user-input");
        .addEventListener('change',function(event){

          // event is the DOM context for the event
          console.log(event.target);
        });
```

`event.target` parameter to the callback contains the element

---
### Example 2: generic method for dealing with an element

What if we have this HTML:
```
<div id="motorbike">honda</div>
<ul>
  <li id="tomatoes">tomatoes</li>
  <li id="lettuce">lettuce</li>
  <li id="bananas">bananas</li>
  <li id="pinapples">pineapple</li>
  <li id="durian">durian</li>
</ul>
```

---

Inside our callback functions, we want a way to access each individual element **value** but in a generic way.

Obviously we would treat our motorbike div differently, but what if we have the generic **produce** element, but they each have a different value inside?

---

```
var dealWithProduce = function(){
  // How do we complete the string?
  // alert(' are yummy!!!');
};
```


```js
// select a set of elements
var listItems = document.querySelectorAll(".li");

// set an event listener on each element
for(var i = 0; i < listItems.length; i++){
    listItems[i].addEventListener("click", function() {
      console.log("hello!");
    }
}
```

We can use `event.target` to reference our **specific** context of the element that was clicked on.
---

# this

### `this` keyword is the current context of the function being executed

```
var dealWithProduce = function(){
  console.log( this );
};
```

**Only the this keyword can be used to set attributes of the element context.**

---

## Pairing Exercise: (15 minutes)
- Start a new `index.html` and `script.js` file
- Copy the HTML and javscript code into their respective files.
- Run it and see the value of `this` and `event` for each time you click.
- Add new elements
- Add a class to the li elements, change selector to class name, run it again
- Add a non-li element with the same class name, run it again
