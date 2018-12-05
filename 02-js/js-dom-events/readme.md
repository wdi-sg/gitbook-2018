#Intro to DOM and Events

---

## Objectives
* Add events to elements in the DOM

---

## Events

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

##### addEventListener

We have different **events**, but now we need a way to attach these events to elements. The solution? Use the `addEventListener` function. This function is called on DOM elements and takes two parameters: an **event type** and a **function**. The event type refers to a "click", "mouseover", or other type of event. The function contains the code that runs when the event occurs.

Also this is the style you should use often.

```js
document.getElementById("myDiv").addEventListener("click", function() {
	//Your code here
})
```

---

Events can only be attached to specific elements. Therefore, when you return a collection such as the result of `document.querySelectorAll` you **CANNOT** simply do this:

```js
document.querySelectorAll("li").addEventListener("click", function() {
	console.log("Click worked");
})
```

---

Instead you must loop through all of the elements and attach an event to each item individually.

```js
var listItems = document.querySelectorAll("li")
for(var i = 0; i < listItems.length; i++){
    listItems[i].addEventListener("click", function() {
    	console.log("Click worked");
    });
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

### Pairing Exercise: (15 minutes)
Create a new html and javascript file.


Using this function:
```
var saySomething = function(){
  alert("YES!");
};
```

And this HTML:
```
<h1>Hello</h1>
<input id="search" />
```

* make sure to use `DOMContentLoaded` or `window.onload`
* set the click event to the h1
* set mouseover to that element
* set keyup or keydown to the input
* try setting some different kinds of events from MDN on these elements.
* try writing some other elements in the HTML file and setting events on them.

[List of Event Types](https://developer.mozilla.org/en-US/docs/Web/Events)

---

# Connecting events to the DOM
## How do we refer to an element that was clicked on?

---
With the tools we have now we can do this:

```
document.getElementById("user-input")
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
document.getElementById("user-input")
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
var listItems = document.querySelectorAll("li");

// set an event listener on each element
for(var i = 0; i < listItems.length; i++){
    listItems[i].addEventListener("click", function() {
      console.log("hello!");
    });
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
